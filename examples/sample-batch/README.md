# Sample Batch — School Uniforms Opinion Article

A synthetic example batch for testing the writing-marker agent end-to-end. Four fictional students at deliberately varied levels.

## What's in here

- `brief.md` — the task brief (the agent's first input)
- `students/anna.txt` — solid Meeting-band piece. All 5 required elements, modest argument, some grammar slips.
- `students/ben.txt` — also solid prose, but **no counterargument**. Should trigger the grade ceiling at Developing (3) despite otherwise-decent writing — the canonical test of the grade-ceiling rule.
- `students/carla.txt` — Better-than-meeting piece. Hook, real concession-then-rebuttal, varied openers, named example. Tests the upper end of the rubric.
- `students/dev.txt` — Emerging piece. Very short, repeated errors, no developed reasons. Tests the lower end and what useful student-facing feedback looks like at this band.

## Expected band distribution (for regression testing)

| Student | Expected band | Why |
|---|---|---|
| Anna | 4 — Meeting | All 5 elements, clear claim, some grammar slips |
| Ben | 3 — Developing (capped) | Strong prose but no counterargument → cap |
| Carla | 5 or 6 — Better than meeting | Hook, real argumentation, varied openers, named example |
| Dev | 2 — Emerging | Single short paragraph per element, frequent errors |

The point of having a synthetic batch isn't to demand the agent hit exactly these bands — different runs may land Anna at 3 or Carla at 6 defensibly. The point is to confirm the agent (a) catches Ben's grade ceiling, (b) does not over-grade Dev, and (c) produces sensible, internally consistent reasoning.

## How to run the agent against this batch

The orchestrator's instructions are in `CLAUDE.md` at the repo root. Point the agent at this folder and the rubric in `rubrics/english-cefr.md`.

## A note on language

The four pieces include realistic ESL-learner errors. They are not models. Do not use them as exemplars in a classroom — they are stress tests for the marker.
