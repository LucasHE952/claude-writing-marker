# Field Guide — Lessons Learned and Edge Cases

A short list of failure modes and edge cases encountered while marking real student batches. Read once per session. Each item is here because it caused, or nearly caused, a wrong mark.

---

## On marking decisions

### The grade ceiling is a ceiling, not a fixed band

When a required element is absent and the grade ceiling engages, the *maximum* overall band is the cap. A piece that is missing a required element **and** weak in other ways can still land below the cap.

The shorthand "grade ceiling = Developing" is misleading if read as "Developing exactly". Read it as "Developing at best". An Emerging piece with a missing feature is still Emerging.

### Within a tier, do not default downward

The English-CEFR rubric describes four descriptor tiers (1–2, 3–4, 5–6, 7) but the band scale has six distinct labels. When a criterion or the overall fits the "5–6" descriptor, pick **5** if the descriptor is barely met and **6** if it is comfortably met. Same for the 3–4 split.

A marker who defaults to the lower number whenever uncertain produces a flat, compressed batch. The defensible move is to look at the descriptor and ask whether the piece is comfortably in it or just barely.

### Same name across students is real

A real batch had two students named William, on different prompts, with no clear visual distinguisher in the photo filenames. They had to be separated and renamed mid-batch. If you see two pieces signed with the same first name and the handwriting style differs, treat them as different students. Ask the teacher to confirm.

### Comparing students is a teacher-facing move, not a student-facing one

Inside teacher marker files, comparison ("strongest in the batch", "more developed than most") is sometimes useful as marker shorthand. Inside *student-facing* feedback notes, comparison is forbidden. Always strip these before producing student versions.

---

## On transcription

### OCR errors flow downstream as student errors

If you misread "new" as "now" and then grade the student's "now" as a tense issue, the error is yours, not theirs. The fix is the discipline of `reading-handwritten-work`: transcribe verbatim, mark `[illegible]` rather than guess, never silently smooth.

### Handwriting messiness is not a basis for deduction

Penmanship is not a writing skill (unless the rubric explicitly says so). A piece you can read with effort is just a piece you can read.

The exception: if the work is so unreadable that whole ideas cannot be retrieved, *Clarity & Organisation* legitimately suffers — because the writing failed to communicate, not because the handwriting was bad. Frame the deduction as a Clarity issue.

### Crossings-out are visible self-edits

Transcribe them as `[crossed out: "X"]`. Credit the revising as positive evidence of drafting behaviour. Grade only the final uncrossed version.

### First-page printed material

Many teacher-provided handwritten batches include a printed rubric, prompt sheet, or instructions at the top of page 1. The student's writing starts further down. The transcription should explicitly note "skipped printed rubric at top of page 1" rather than silently include or exclude it.

---

## On batch handling

### Drift is real even in careful marking

Single-pass marking, even done by an attentive marker, drifts. The marker becomes tougher or looser by the time they reach student 25 than they were at student 5. This is what `calibrating-a-batch` exists to surface.

A clean calibration report ("no significant inconsistencies") is a valid output. Don't manufacture inconsistencies to flag.

### Batches under ~6 students are too small to calibrate

Cross-student comparisons require enough students to detect patterns. If the batch is 4 students, skip the calibration stage and note it in the synthesis.

### Student-facing notes are not produced by default

The base marking workflow (Stages 1–4) produces teacher-facing marker files, a calibration report, and a batch synthesis. Student-facing notes (Stage 5) only run if the teacher explicitly asks. The reason: student-facing notes communicate the band to the student, and a teacher should approve the marks before they are communicated.

---

## On feedback

### Quote the student's own words inside praise

"Your line *'such a cold day'* set the mood instantly" beats "good atmosphere". The first builds trust because it proves the marker read the piece; the second is a template.

### Action verbs in "Even Better If"

"Tense inconsistency" is a diagnosis. "Try writing all your action lines in present tense" is an action. Students follow actions, not diagnoses. Always phrase EBI as a try-this.

### Two or three EBI bullets, not five

A piece can have ten things to improve. The student-facing note picks the two or three highest-impact ones. The rest go in the teacher marker file and inform the teacher's lesson planning, not the student's next attempt.

### Language-level matters for ESL learners

Idioms in feedback ("hook the reader", "lift the energy", "spelling pass") read fluently to native speakers and confuse ESL learners. For B1 audiences, swap to literal phrasing ("make the reader want to keep reading", "make the writing feel stronger", "check the spelling").

---

## On rubrics

### The teacher's rubric is the rubric

Don't blend rubrics. Don't import rules from a rubric you remember from a different session. If the teacher supplied `rubrics/ib-humanities-essay.md`, you apply *that* rubric — including its specific decisions (e.g. whether spelling counts as Accuracy).

### Rubric metadata controls the grade-ceiling band

The orchestrator reads `grade_ceiling_band` from the rubric's metadata block to know what "missing a required element" means in that rubric's scale. A 1–4 rubric typically uses 2; a 1–7 rubric typically uses 3. Don't assume; read the metadata.

### Rubrics evolve; the discipline doesn't

The marking discipline rules in `CLAUDE.md` (transcribe before judging, credit what's present, etc.) apply to every rubric. When a new rubric is added, those rules carry over unchanged. Resist the urge to bake rubric-specific tricks into the discipline.

---

## Known gaps in v0.1

- No cheating / AI-content detection. Out of scope.
- No grade-book or LMS integration. Out of scope.
- No live collaboration with multiple markers. The "second marker" role is played by `calibrating-a-batch` operating on a single marker's output, not by a human second marker.
- No support for non-text rubrics (e.g. audio, video, presentation). The agent is built for written work.
- No automatic rubric inference. If the teacher does not supply a rubric, the agent stops and asks. It does not invent one.
