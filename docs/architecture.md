# Architecture — Workflow and Skill Contracts

This document defines the stages of the writing-marker workflow, the input/output of each stage, and which skill is invoked where. Skills are built against the contracts on this page; the orchestrator (`CLAUDE.md`) is built to wire them together by dispatching subagents.

**The orchestrator never reads student work, transcriptions, or full marker files directly.** Every content-touching stage is run by a subagent the orchestrator dispatches. See `docs/subagent-architecture.md` for the full pattern.

If you are adding or modifying a skill, update this document first so the contract is explicit before the skill is implemented.

---

## Inputs the teacher provides

1. **Task brief** — a markdown file describing the prompt, required structural elements (if any), expected level, and which rubric to use. See `examples/sample-batch/brief.md` for the shape.
2. **Rubric** — a markdown file with band scale, criteria, descriptors, synthesis rules, and metadata (grade-ceiling band, scale type). See `rubrics/english-cefr.md` for the format.
3. **Student work** — one file per student. Two input shapes:
   - **Typed / digital** (default): `.txt`, `.md`, `.docx`. The fast path — no transcription step.
   - **Handwritten**: `.jpg`, `.png`, `.pdf`. Multi-page work named with a numeric suffix (e.g. `alex_1.jpg`, `alex_2.jpg`). Triggers the `reading-handwritten-work` skill.

The orchestrator should refuse to begin if any of these are missing or ambiguous and ask the teacher to supply them.

---

## Stage 1 — Setup

**Input:** the three user-provided files above.
**Output:** an in-memory plan: list of students, list of expected files per student, identified rubric, identified required elements.

**No skill invoked.** The orchestrator does this directly.

The orchestrator must verify:
- Every student has at least one file.
- Filenames suggest the same student for multi-page work (e.g. `alex_1`, `alex_2` clearly belong together).
- The rubric file is parsable (band scale present, criteria present, grade-ceiling band present).

If any of these checks fail, stop and ask the teacher.

---

## Stage 2 — Per-student marking (subagent-delegated)

**Dispatched by:** the orchestrator, as one or more parallel marking subagents. The orchestrator does not run this stage in its own context. Partition sizing in `docs/subagent-architecture.md`.

Each marking subagent, for each student in its partition:

### 2a. Transcription (if handwritten)

**Skill:** `reading-handwritten-work` (invoked by the marking subagent).
**Input:** one or more image files belonging to a single student, plus the task brief (so it knows what to skip — e.g. printed rubrics, prompts).
**Output:** a *transcription block* — verbatim text with `[illegible]` markers where appropriate, plus a *transcription confidence note* flagging anything the subagent needs to know (page-order suspicion, mismatched-student suspicion, image-quality issues, large illegible runs).

If the input is already text, this skill is skipped.

### 2b. Scoring against the rubric

**No skill invoked.** The marking subagent does this directly with the rubric file in context.

**Input:** the transcription, the rubric, the task brief.
**Output:** a per-criterion score, plus required-element presence/absence, plus an overall band synthesised from the criterion scores (following the rubric's synthesis rules and grade-ceiling behavior).

This stage applies the marking discipline rules from `CLAUDE.md`: quote evidence verbatim, credit what's present, distinguish authorial choice from error, enforce the brief's required elements as a floor.

### 2c. Per-student marker file

**No skill invoked.** The marking subagent writes the file directly.

**Output file:** `private/{batch}/{student}.txt` (in real runs) or `examples/sample-batch/results/{student}.txt` (in test runs). Plain text, no markdown — teachers read these in Notes, Word, or email. Format follows the per-student marker file shape below.

### Subagent return

Each marking subagent returns a one-line summary per student to the orchestrator: `{name} {band} ({label})`. It flags any student it could not mark. The orchestrator collates these summaries; the full marker files stay on disk and are read in Stages 3 and 4 by a different subagent.

---

## Stage 3 — Batch calibration (subagent-delegated)

**Dispatched by:** the orchestrator, as one calibration subagent. The orchestrator does not load the marker files into its own context.

**Skill:** `calibrating-a-batch` (invoked by the calibration subagent).
**Input:** all per-student marker files from Stage 2, the rubric, the task brief.
**Skip condition:** if the batch has fewer than 6 students. Cross-student calibration requires enough students to detect patterns; with 4–5 marker files there isn't enough signal. The orchestrator notes the skip in the Stage 4 synthesis.
**Output:** a *calibration report* (`{batch}-calibration.txt`) flagging:
- Pairs/triples of students with similar-quality work but different bands.
- Drift across the batch (e.g. first 10 students vs last 10).
- The same issue weighted differently across students.
- Suggested band adjustments (always as recommendations, not silent rewrites).

### Subagent return

The calibration subagent saves the report to disk and returns a one-paragraph verdict to the orchestrator. The orchestrator may revise marker files based on this report, but only after surfacing the changes to the teacher.

---

## Stage 4 — Batch synthesis (subagent-delegated)

**Dispatched by:** the orchestrator, as one synthesis subagent. The orchestrator does not load the marker files into its own context.

**Skill:** `synthesizing-batch-issues` (invoked by the synthesis subagent).
**Input:** all per-student marker files, the task brief, the batch size (so the skill can adapt thresholds).
**Output:** a *teaching priorities document* (`{batch}-synthesis.txt`) with:
- Patterns appearing in 4+ students (or 2+ when batch size is under 10), with counts and quoted examples.
- Required-element gaps across the batch.
- Strengths to protect.
- A ranked teaching punch-list for the next 2–3 lessons.

### Subagent return

The synthesis subagent saves the document to disk and returns the ranked punch-list to the orchestrator.

---

## Stage 5 — Student-facing conversion (subagent-delegated, optional)

Only runs if the teacher asks for it. **Dispatched by:** the orchestrator, as one subagent per marker file, all dispatched in parallel.

**Skill:** `writing-student-facing-feedback` (invoked by each student-facing subagent).
**Input:** one per-student marker file (TXT), the configured target language level (default: B1).
**Output:** a student-facing plain-text file at `private/{batch}/student-versions/{student}.txt`.

Rules baked into the skill: no peer comparisons, WWW/EBI structure, max 2–3 EBI bullets, no rubric jargon, configured language level, student's quoted work preserved verbatim.

### Subagent return

Each subagent returns a one-line confirmation to the orchestrator. The student-facing files are on disk.

---

## Data shapes

### Per-student marker file (plain text, `.txt`)

```
{Student name}
Overall: {score} / {scale_max} — {band label}

Rubric: {rubric file name}
Brief: {brief file name}


TRANSCRIPTION

{verbatim text, [illegible] for unreadable spans}


REQUIRED ELEMENTS

(Only if the brief defines them.)
- {Feature 1}: PRESENT — "quoted example"
- {Feature 2}: ABSENT
- Grade ceiling (if any absent): max overall = {band label}


CRITERION SCORES

{Dimension 1}
- {Criterion}: {score}
- {Criterion}: {score}

{Dimension 2}
- {Criterion}: {score}


OVERALL: {score} / {scale_max} — {band label}

{One to two sentences explaining the synthesis. If grade ceiling engaged but synthesis is below it, state both: "Grade ceiling engaged at 3; synthesis lands at 2 due to..."}


EVIDENCE

- "{quoted phrase}" — {what it shows, against which criterion}


WHAT'S WORKING

- ...


WHAT TO WORK ON

- ...
```

### Calibration report shape (plain text, `.txt`)

```
CALIBRATION REPORT — {batch name}

Batch size: {N} students
Rubric: {rubric file name}


CROSS-STUDENT INCONSISTENCIES

- Students A and B were given different bands ({band A} vs {band B}) but the evidence appears similar.
  - A: "{quoted issue}" → {how it was weighted}
  - B: "{quoted issue}" → {how it was weighted}
  - Recommendation: ...


DRIFT ACROSS THE BATCH

- ...


SAME ISSUE, DIFFERENT WEIGHT

- ...


SUMMARY

{One paragraph: are the marks defensible as a batch? What needs revision?}
```

### Batch synthesis shape (plain text, `.txt`)

```
WRITING ISSUES — {batch name}

Batch: {N} students
Task: {one-line task description}
Rubric: {rubric file name}


REQUIRED-FEATURE GAPS

- ...


RECURRING LANGUAGE / ACCURACY PATTERNS

- {pattern} ({N}/{total}): "{quote}", "{quote}"
- ...


GENRE-SPECIFIC ISSUES

- ...


STRENGTHS OBSERVED BROADLY

- ...


TEACHING PUNCH-LIST — NEXT 2–3 LESSONS

1. ...
```

---

## Decisions baked into this architecture

1. **The orchestrator never reads student work, transcriptions, or full marker files.** Every content-touching stage is delegated to a subagent. The orchestrator's context holds the rubric, the brief, teacher preferences, and the *reports* coming back. See `docs/subagent-architecture.md` for the rationale.
2. **The marking subagent owns the rubric application.** Scoring is not a separate skill because it requires holding the full rubric, the task brief, and the transcription in context together. Splitting it out would force passing too much state.
3. **Transcription and student-facing conversion are skills** because they are self-contained, independently invokable, and encode discipline that is easy to break. They are invoked by subagents, not by the orchestrator.
4. **Calibration and synthesis are skills** because they operate on the batch as a whole, take all marker files as input, and produce one report each. They are invoked by subagents the orchestrator dispatches.
5. **The four skills do not call each other.** They are all invoked by subagents the orchestrator dispatches.
6. **Grade-ceiling logic lives in the rubric, not the orchestrator.** Different rubrics may set different grade-ceiling bands. The marking subagent reads the band from the rubric metadata.
7. **All real student data lives in `private/`.** Subagents and the orchestrator never write to anywhere else when running on real batches. The `examples/` folder is for synthetic test fixtures only.

---

## What this architecture does not do

- It does not auto-detect cheating, AI-generated content, or plagiarism.
- It does not score on a scale the rubric doesn't specify (no percentages, no GPAs).
- It does not write to a learning management system or grade book.
- It does not refine the rubric itself based on what it sees in the batch.

These could be future work, but are deliberately out of v0.1 scope.
