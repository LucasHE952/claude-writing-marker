# Scaling Large Batches — Parallel Subagent Pattern

When the batch is larger than a single marking pass can comfortably handle, fan out across parallel subagents. This document describes when to fan out, how to partition the work, and what stays serial.

## Rules of thumb

| Batch size | Recommendation |
|---|---|
| Under 12 | Single pass. Mark serially in one session. |
| 12–25 | Single pass is fine; fan out only if response latency matters. |
| 25–60 | **Fan out.** Single-pass marking starts to drift; subagents in parallel are faster and the drift across the cohort is reduced. |
| 60+ | Fan out, and consider larger partitions per agent (8–12 students each) to keep the number of subagents manageable. |

The numbers are guidance, not law. The deciding factor is whether the marker's attention and consistency hold across the whole batch in one pass. A marker reading carefully tires after about 20 students; a parallel set of subagents each marking 8 are individually fresh.

## Partition strategy

Split alphabetically by filename, in equal-sized chunks. Avoid splitting by criterion (e.g. "agent A marks Language, agent B marks Message") — every agent must score all criteria for every student it sees, otherwise calibration becomes impossible.

For a 40-student batch, a typical partition is 5 agents × 8 students:

- Agent 1: students 1–8
- Agent 2: students 9–16
- Agent 3: students 17–24
- Agent 4: students 25–32
- Agent 5: students 33–40

Each agent is dispatched in parallel (single `Agent` invocation with multiple tool calls in one message), runs to completion independently, and writes marker files to the same target directory.

## What each subagent receives

A self-contained brief, every time. The subagent has no context from the orchestrator beyond what you pass it. Include:

1. The path to `CLAUDE.md` (the orchestrator's discipline).
2. The path to the rubric file (`rubrics/english-cefr.md` or similar).
3. The path to the task brief (`{batch}/brief.md`).
4. The list of student files this agent is responsible for, with their full paths.
5. The target output directory for marker files.
6. Optionally: a one-line description of the cohort level, so the agent can calibrate its expectations.

**Do not** ask one subagent to read another subagent's output. They run in isolation by design. Cross-agent calibration is done after, in Stage 3.

## What stays serial

- **Stage 3 — Calibration.** Cannot parallelize. The calibration skill needs the *full* set of marker files as input to detect cross-student inconsistencies.
- **Stage 4 — Synthesis.** Same reason. Pattern-mining and counting requires the whole batch.
- **Stage 5 — Student-facing conversion.** *Could* parallelize, but the conversion is fast (no transcription, no synthesis) so the benefit is small. Run serially unless the batch is very large.

A typical timeline for a 40-student batch:

```
T+0min   Dispatch 5 marking subagents in parallel
T+5min   First subagents complete; orchestrator monitors
T+10min  All 5 subagents complete
T+10min  Stage 3 — Calibration (serial, one pass)
T+12min  Stage 4 — Synthesis (serial, one pass)
T+14min  Optionally: Stage 5 — student-facing conversion (serial or parallel)
```

vs. single-pass:

```
T+0min   Begin marking student 1
T+40min  Complete student 40 (and drift detected in Stage 3)
T+42min  Synthesis
```

The parallel pattern is ~3x faster *and* produces more consistent marks because each subagent is fresh.

## Subagent output contract

Each subagent reports back with a one-line summary at completion, in the form:

```
Alex 4 (Meeting), Brianna 3 (Developing, grade-ceiling), Casey 5 (Better than meeting), ...
```

The orchestrator collates these into a batch distribution. The full marker files are on disk; the one-line summaries are for the orchestrator and the teacher to confirm everything ran cleanly.

If a subagent fails (cannot read a file, hits an unexpected case, decides it cannot mark a piece), it should stop and report that — not invent a band.

## When parallelization makes things worse

- **Tiny batches.** For under 12 students, the overhead of partitioning and dispatching subagents exceeds the time saved.
- **Batches where calibration is the goal.** If the whole point of the exercise is internal consistency (e.g. a high-stakes assessment where every band must be defensible against every other), single-pass marking with one careful marker is actually preferable. Calibration is a backstop, not a substitute.
- **Mixed rubrics within a batch.** Don't fan out if different students are being marked against different rubrics — the subagents would each need the rubric switching logic in context, which is brittle. Mark by rubric in serial sub-batches instead.

## Implementation note

The orchestrator (`CLAUDE.md`) checks the batch size at the start of Stage 2 and decides whether to fan out. The fan-out decision is the orchestrator's; the marking discipline is each subagent's.

When in doubt, fan out. The cost of an extra subagent is small. The cost of mid-batch drift in a single pass is real.
