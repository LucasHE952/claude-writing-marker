# Writing Marker

A general-purpose Claude agent for marking student writing — handwritten or typed, in any subject where students compose longer pieces of writing (English, humanities, social studies, science write-ups).

You bring a **task brief** and a **rubric**. The agent marks each piece, calibrates across the batch for consistency, writes student-facing feedback at a configurable language level, and produces a teaching-priorities synthesis across the cohort.

## What this is not

- Not a model. The agent uses Claude (any current model) to do the actual reading and judging — this repo contains the *discipline*: the prompts, the skills, and the workflow that surrounds the model so the marking is consistent and defensible.
- Not a grader for short-answer or fill-the-blank work. The agent is built for longer compositions where a holistic band makes sense.
- Not a replacement for a teacher's professional judgment. Use it to draft feedback that a teacher reviews and signs off on.

## Quick start

You need three things:

1. **A task brief** — what the students were asked to do (prompt, required structural elements if any, expected level).
2. **A rubric** — the criteria, the band scale, the descriptors per band. Use one of the rubrics in `rubrics/` or write your own.
3. **The student work**:
   - **Typed essays** (the common case) — a folder of `.txt`, `.docx`, or `.md` files, one per student.
   - **Handwritten work** — photos of pages (`.jpg`, `.png`, `.pdf`), one folder per student or filenames numbered per page (`alex_1.jpg`, `alex_2.jpg`).

Then point the agent at the batch. The agent's instructions in `CLAUDE.md` describe the workflow stage by stage.

A worked example lives in `examples/sample-batch/` — synthetic typed-essay submissions plus the brief and rubric used to mark them. Use this as a quick check before pointing the agent at real student work.

For a flow chart of the full workflow, see [`docs/workflow.md`](docs/workflow.md).

## What the agent produces

For each batch, the agent writes plain-text output files that teachers can open in any editor (Notes, Word, email — no markdown rendering required):

- One **per-student marker file** (`.txt`) with the band, scores against each criterion, evidence, strengths, and what to work on.
- One **calibration report** that audits the batch for internal consistency (drift, inconsistent banding).
- One **batch synthesis** — a teaching-priorities document with patterns across the cohort and a ranked punch-list for the next 2–3 lessons.
- Optionally: **student-facing feedback notes** at a configurable language level (default CEFR B1), with no peer comparisons and no rubric jargon.

The marker files, calibration, and synthesis are output as `.txt` because the people who read them are teachers, not engineers. The agent's own configuration (`CLAUDE.md`, skills, rubrics, task briefs) stays in markdown because the AI reads those.

## How the agent stays consistent

The agent uses an orchestrator-plus-subagents pattern. The orchestrator never reads student work directly — it dispatches subagents to mark, calibrate, synthesise, and (optionally) generate student-facing feedback. Each subagent runs in a fresh context.

This matters most for calibration: a marker that has just spent its context window marking 20 students will calibrate worse than a fresh subagent that reads only the resulting marker files. The pattern is the same regardless of batch size — small batches and large batches both use subagents; large batches just use more of them in parallel. See `docs/subagent-architecture.md` for the details.

## Repo layout

```
writing-marker/
├── CLAUDE.md                          # The agent's instructions — discipline + workflow
├── README.md                          # This file
├── LICENSE                            # MIT
├── skills/
│   ├── reading-handwritten-work.md
│   ├── writing-student-facing-feedback.md
│   ├── calibrating-a-batch.md
│   └── synthesizing-batch-issues.md
├── rubrics/
│   ├── english-cefr.md                # Default 1–7 holistic rubric, English-leaning
│   ├── ib-humanities-essay.md         # IB-style 1–7 essay rubric
│   └── generic-argumentation.md       # Subject-agnostic argument rubric
├── examples/
│   ├── sample-batch/                  # Synthetic worked example
│   ├── script-brief/                  # Example task brief (script writing)
│   └── opinion-article-brief/         # Example task brief (opinion article)
├── docs/
│   ├── architecture.md                # Workflow and skill contracts
│   ├── subagent-architecture.md       # Why and how the orchestrator delegates everything to subagents
│   ├── workflow.md                    # Flow chart of the five stages
│   └── field-guide.md                 # Known issues, lessons learned, edge cases
└── private/                           # Real student data goes here (gitignored)
```

## License

MIT. See `LICENSE`.

