---
name: calibrating-a-batch
description: Read all per-student marker files for a batch and produce a calibration report flagging inconsistencies — different bands for similar work, drift between early and late students, the same issue weighted differently across students. Recommends adjustments but never silently rewrites.
---

# Calibrating a Batch

You are the second pair of eyes on a marked batch. Each per-student marker file was written in isolation. Your job is to check whether the marks hold together as a batch — whether two pieces of similar quality were given similar bands, whether the rubric was applied consistently from the first student to the last, whether the same kind of error was treated the same way each time.

You produce **a report**. You do not silently change any per-student marker file. The orchestrator may revise marks based on your report, but only after surfacing your recommendations to the teacher.

## When to invoke

- After all per-student marker files for a batch are written.
- Before any batch synthesis (the synthesis is more useful once the marks are calibrated).
- Before any student-facing conversion (a band shouldn't change after the student has seen it).

## Input contract

- All per-student marker files in a batch (typically 10–60 files).
- The rubric file (so you know what each band is supposed to mean).
- The task brief (so you know what the required elements were).

## Output contract

- A single calibration report at `{batch}-calibration.txt` containing four sections, plus a summary.
- Plain text, no markdown syntax — teachers read this in Notes, Word, or email. Use ALL CAPS section headers and `-` bullets.

If the batch is too small to calibrate (under ~6 students), say so in the report and stop. With fewer than 6 marker files there isn't enough signal to detect drift. The orchestrator should normally skip this skill entirely for small batches, but if invoked, return a short report saying so rather than manufacturing inconsistencies.

---

## What to look for

### 1. Cross-student inconsistencies

Two (or more) students whose work shows similar quality but received different bands. The most damaging failure mode of single-pass marking.

To find these, group students by their overall band, then look for pairs *across* adjacent bands where the evidence sections look similar in shape:

- Same required elements present, same overall structure, similar accuracy issues → should be near each other in band.
- One student flagged for a serious issue (e.g. "comma-splice run-on entire paragraph") that another student also has but wasn't flagged for → inconsistent attention.
- Two pieces with the same grade-ceiling trigger but different overall bands → suspicious.

Flag with concrete quotes:

```
Students Anna (band 4) and Ravi (band 3) both have:
- All 5 required elements present.
- Subject-verb agreement issues throughout.
- One developed example per reason.
The evidence sections are very similar. The band difference is hard to defend.
Recommendation: re-read both. Consider lifting Ravi to 4 or lowering Anna to 3.
```

### 2. Drift across the batch

Compare the bands and severity of comments in the first ~10 students against the last ~10. Common drift patterns:

- **Tougher early, looser late.** Marker fatigue: the criterion becomes more permissive as the marker gets tired.
- **Looser early, tougher late.** Calibration adjusts as the marker sees the cohort range — the early students may have been over-credited.
- **Specific issues flagged early, missed late.** A spelling error called out for student 3 but ignored by student 23.

To check: sample two early marker files and two late marker files, read them side by side, look for tone shifts and severity shifts. Quote specific lines.

### 3. Same issue weighted differently

Pick 2–3 errors that recur across the batch (e.g. "missing counterargument", "subject-verb agreement with collective subjects", "run-on sentences"). For each, list the students who have the issue, and the band they received.

If "missing counterargument" appears in 5 students and 4 are capped at 3 but the fifth is at 4, surface the discrepancy.

### 4. Recommendations

For each flagged inconsistency: a recommended action. Never write "change X from 4 to 3". Write "consider lifting X to 4 *or* lowering Y to 3 — both pieces should land in the same band".

Recommendations should be paired alternatives, not one-sided demands. The marker (or the teacher) decides which way to resolve them.

---

## What NOT to do

### Do not silently rewrite

You produce a report. You never edit a per-student marker file. The orchestrator handles revisions after the teacher has reviewed your recommendations.

### Do not invent issues to flag

If the batch is genuinely consistent, say so. Single-pass marking is often fine; calibration is an audit, not a witch-hunt. A clean report ("no significant inconsistencies detected — see summary") is a valid output.

### Do not second-guess the rubric

You check whether the rubric was applied consistently, not whether the rubric is good. If the rubric says spelling counts as Accuracy and the marker did apply that, don't flag it just because you would have weighted spelling differently.

### Do not re-mark

You are not regrading each student. You are checking the batch's internal coherence. Treat the existing bands as the starting point and look for where they don't hang together. If you find yourself re-reading the student's transcription to re-score it, you are doing the wrong job.

---

## Output format (plain text, `.txt`)

```
CALIBRATION REPORT — {batch name}

Batch size: {N} students
Rubric: {rubric file name}
Marked between: {first marker file timestamp} and {last marker file timestamp} (if available)


1. CROSS-STUDENT INCONSISTENCIES

{One paragraph per pair/triple, with quoted evidence and a paired-alternative recommendation. If none found, "No cross-student inconsistencies detected."}


2. DRIFT ACROSS THE BATCH

{One paragraph describing whether tone/severity shifted from early to late, with quoted examples. If no drift, say so.}


3. SAME ISSUE WEIGHTED DIFFERENTLY

{Per recurring issue: list of students affected, their bands, any inconsistency in how the issue was weighted. If consistent, say so.}


4. SUMMARY

{One short paragraph. How many issues flagged. Overall: are the marks defensible as a batch? What is the single highest-priority revision, if any?}
```

---

## Worked example (excerpt)

```
## 1. Cross-student inconsistencies

**Anna (band 4) and Ravi (band 3).** Both pieces include all five required structural elements. Both have ~6–8 subject-verb agreement errors. Both develop their reasons with one example each.
- Anna evidence: "When everyone wears the same thing, no one feels embarassed about their clothes" (one example, modest development).
- Ravi evidence: "When students don't have uniform, some kids show off their expensive clothes" (one example, modest development).
The band gap is not supported by the evidence sections. Recommendation: re-read both and either lift Ravi to 4 or lower Anna to 3.

**Carla (band 6) and Jay (band 5).** Carla has one named statistic and a real concession-then-rebuttal. Jay's piece is comparably structured but with no named example. The 5/6 split is defensible here — the evidence section does support Carla being one tier higher. No action recommended.

## 2. Drift across the batch

No clear drift detected. Comment severity for spelling errors is consistent across early and late students (Anna and Tara — both early, both with similar misspellings — were treated similarly to Yusuf and Wendy who came later).

## 3. Same issue weighted differently

**"Missing counterargument"**: triggers grade ceiling for Ben (3) and Tara (3). No inconsistency.

**"Two reasons that restate each other"**: flagged for Anna (band 4), Liam (band 4), and Mei (band 5). Mei was rewarded for stronger language at the surface level — the gap is defensible.

## 4. Summary

One significant inconsistency (Anna / Ravi pair). One small inconsistency (resolved on re-read of the evidence). No batch-wide drift. The marks are largely defensible as a batch; the Anna / Ravi pair should be resolved before student-facing notes are produced.
```
