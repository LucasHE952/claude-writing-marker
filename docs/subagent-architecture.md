# Subagent Architecture — Why the Orchestrator Doesn't Read Student Work

The writing-marker agent has a strict rule: **the orchestrator never holds student work in its context window.** Every stage that reads transcriptions, page images, or marker files is delegated to a subagent. The orchestrator's context holds the rubric, the task brief, the teacher's preferences, and the *reports* coming back from subagents — never the bulk content itself.

This is not a speed optimization. It is a quality protection.

## Why

A marking decision made on a saturated context is a worse decision than a marking decision made on a fresh one. By the time an orchestrator has read 20 transcriptions, written 20 marker files, and then tried to read all 20 marker files again to calibrate and synthesise, its judgement is operating in a partially-degraded window. The marks at the start of the batch are made by a sharper agent than the marks at the end. Calibration done in the same context as marking is calibration done by an agent that has already committed to the marks.

Subagent delegation solves this by giving each stage a fresh context:

- A **marking subagent** reads the rubric, the brief, and a small group of students' work. It writes marker files. It returns a one-line summary per student. It is then discarded.
- The **calibration subagent** reads all the marker files at once, but it has never seen the original student work and has no commitment to the marks it is auditing.
- The **synthesis subagent** reads all the marker files and produces a teaching-priorities document, again without holding any of the original work or any prior reasoning.
- **Student-facing subagents** each read one marker file and produce one student-facing note.

The orchestrator coordinates. It does not read student work, transcriptions, or full marker files.

## Which stages delegate, and how

| Stage | Subagents dispatched | What the orchestrator receives back |
|---|---|---|
| 2 — Marking | 1 subagent for ≤10 students; fan out above (8–12 students per subagent) | One-line summary per student; marker files saved to disk |
| 3 — Calibration | 1 subagent reads all marker files | Calibration report saved to disk; one-paragraph verdict returned inline |
| 4 — Synthesis | 1 subagent reads all marker files | Teaching-priorities document saved to disk; ranked punch-list returned inline |
| 5 — Student-facing (if requested) | Per-student subagents, parallel | One-line confirmation per student; student-facing files saved to disk |

Stages 3, 4, and 5 each use a subagent **regardless of batch size**. The dispatch overhead is small; the protection of the orchestrator's context is universal.

## Partitioning Stage 2 for large batches

When the batch is large enough to need multiple marking subagents, partition alphabetically by filename in equal chunks. Avoid splitting by criterion — every subagent must score all criteria for every student it sees, or calibration becomes impossible.

| Batch size | Partition |
|---|---|
| ≤10 | 1 subagent |
| 11–25 | 2–3 subagents (8–10 students each) |
| 26–60 | 4–6 subagents (8–12 students each) |
| 60+ | 6+ subagents (8–12 students each) |

Dispatch all marking subagents in parallel — a single message with multiple `Agent` tool calls. Each subagent is self-contained and runs to completion independently, writing marker files to the same target directory.

## What each subagent receives

A self-contained brief, every time. The subagent has no context from the orchestrator beyond what you pass it. Include:

1. The path to `CLAUDE.md` (the orchestrator's discipline). The subagent must read this before marking.
2. The path to the rubric file (`rubrics/english-cefr.md` or similar).
3. The path to the task brief (`{batch}/brief.md`).
4. The list of student files this subagent is responsible for, with full paths.
5. The target output directory for marker files.
6. Optionally: a one-line description of the cohort level, so the subagent can calibrate its expectations.

**Do not** ask one subagent to read another subagent's output. They run in isolation by design. Cross-agent calibration is done in Stage 3 by a separate subagent that has the full set of marker files as input.

## What each subagent returns

A short, structured report — never the marker file contents. The orchestrator's context only needs to know:

- Which students were marked, and what band each received.
- Any students the subagent could not mark (a flag, not a guessed grade).
- Where the marker files are on disk.

Long-form content stays in the marker files. The orchestrator reads them only if a teacher question requires it, and even then prefers to dispatch a fresh subagent to inspect a specific file rather than load it into its own context.

## Stage 3 and Stage 4 — single subagent each

Calibration and synthesis cannot parallelize because both need the full set of marker files as input. But neither should run in the orchestrator's context either. Dispatch a single subagent for each:

- **Calibration subagent.** Receives: paths to all marker files in the batch, the rubric, the brief. Invokes the `calibrating-a-batch` skill. Saves a calibration report to `{batch}-calibration.txt` and returns a one-paragraph verdict inline.
- **Synthesis subagent.** Receives: paths to all marker files, the brief, the batch size. Invokes the `synthesizing-batch-issues` skill. Saves a teaching-priorities document to `{batch}-synthesis.txt` and returns the ranked punch-list inline.

The orchestrator passes these reports back to the teacher.

## Stage 5 — per-student subagents in parallel

Student-facing conversion is embarrassingly parallel. Each subagent reads one marker file, invokes the `writing-student-facing-feedback` skill, and produces one student-facing file. Dispatch them all at once.

## When the subagent pattern works against you

- **Mixed rubrics within a batch.** If different students are being marked against different rubrics, fan-out becomes brittle — each subagent would need rubric-switching logic in context. Mark by rubric in serial sub-batches instead.
- **A teacher question about one specific marker file.** Don't dispatch a subagent for a one-off lookup; just read the file. The pattern is about bulk reads, not single ones.

## Implementation note

This pattern is enforced in `CLAUDE.md`. The orchestrator's instructions name subagent delegation as a `MUST` at each marking stage. A future contributor who is tempted to "just read the marker files inline" should understand the trade: it speeds up the immediate response, but degrades the orchestrator's context for everything that follows in the same session.
