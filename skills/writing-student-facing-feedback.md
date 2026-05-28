---
name: writing-student-facing-feedback
description: Convert a teacher-facing per-student marker file into a student-facing plain-text feedback note. Selects the highest-impact issues, removes rubric jargon and peer comparisons, and adapts language to a configurable level. Use when the teacher wants feedback returned to students directly.
---

# Writing Student-Facing Feedback

You are converting a teacher's marker file (the source of truth) into a feedback note the student will read. The structure is the same across every student. The tone is constructive, second-person, and selective: you choose the 2–3 highest-impact issues, not the full list.

## When to invoke

- After per-student marker files have been written.
- Only if the teacher has asked for student-facing versions.
- Once per student.

## Input contract

- One per-student marker file (the teacher version).
- A `target_language_level` parameter (default: CEFR B1). Other valid values: `A2`, `B2`, `C1`, or `native` (no language-level adjustments).

## Output contract

- One plain-text file with the same base name as the source, extension `.txt`.
- No markdown syntax.
- Under 250 words.

---

## Core discipline rules

### 1. No comparison to other students — under any framing

Never write statements that rank a student against peers or the cohort. Praise the work on its own merits.

| Don't write | Write instead |
|---|---|
| "Your article is the strongest in the group." | "Your article is well-developed and well-organized." |
| "The best opening in the batch." | "Your opening makes the reader want to keep reading." |
| "More developed than most." | "You developed each idea with care." |
| "One of the top pieces." | "This is a strong piece." |

Even soft comparisons ("among the best", "stands out", "more than most students") are out. The test: if you remove the comparison clause and the praise still stands on its own, the sentence is fine.

This rule covers **all** comparison phrasings, including ones like "most students do not try this" or "students at this level rarely…" — those are peer comparisons too.

### 2. Pick the top 2–3 issues only

The teacher marker file captures every issue. The student version *selects*. Choose the patterns whose repair would lift the next piece the most. Leave the rest. Overwhelming the student with a long list of errors is the failure mode.

### 3. Address the student in second person

"You", "your article", "your script". Never "the student". Use the student's name once, in the opening line, in title case (not all caps even if the source filename is).

### 4. Specific over generic — quote the student

Pull the student's own words into the praise. "Your line *'such a cold day'* set the mood" beats "good atmosphere". Generic praise reads like a template and lowers the trust the student places in the rest of the note.

### 5. Actions, not diagnoses, in EBI

| Diagnosis (don't) | Action (do) |
|---|---|
| "Tense inconsistency in action lines." | "Try writing all your action lines in present tense." |
| "Subject-verb agreement errors." | "When the subject is plural, the verb has no 's'. 'Young people don't' — not 'doesn't'." |
| "Required element missing: counterargument." | "Next time, add a paragraph that explains what someone who disagrees with you might say." |

### 6. No rubric jargon

The student does not see the rubric. Never write *criterion*, *grade ceiling*, *Vocabulary 4*, *Text Type Conventions*, *required element absent*, *gate*, *cap*. These are teacher words.

### 7. Grade-ceilingped pieces

If a required structural element is missing and triggered a grade ceiling, fold the missing piece into an EBI bullet as a "next time" instruction. Do not mention grade ceilings, gates, or grading mechanics. The band line at the top already reflects the cap; the student doesn't need to read about the machinery.

### 8. Constructive tone

- Warm but not falsely cheerful.
- No "you must" or "you need to". Use "try", "next time", "watch for", "make sure".
- Don't moralise. Comment on the writing, not the student's character or effort.

---

## Language-level adaptation

### CEFR B1 (default)

For lower-intermediate ESL learners (e.g. G7–G9 EAL classrooms).

- Short sentences. Aim for 8–18 words. Break anything longer.
- One idea per sentence.
- High-frequency vocabulary. Concrete over abstract.
- Simple connectives: *and, but, so, because, when, if, then*.
- **No idioms. No metaphors used as feedback.** (Inside the student's own quoted writing, idioms are fine — those are theirs.)
- **No opaque phrasal verbs.** Transparent ones (*find out, write down, look at, give up*) are fine.

Common swaps for B1:

| Don't use | Use instead |
|---|---|
| hook the reader | make the reader want to keep reading |
| lift the energy | make the writing feel stronger |
| land even harder | have an even stronger effect |
| the real thing | very good / strong |
| stay there (tense) | only use past tense / only use present tense |
| spelling pass | check the spelling |
| tighten | make shorter |
| genre-aware | you understand this kind of writing |
| cinematic | like a film |
| evocative | strong; helps the reader picture it |
| at the right moment | at a good place |
| nail / nailed | get right |
| lean into | use more of |
| pay off | give a good result |

### CEFR A2

Even shorter sentences (5–12 words). Almost no subordinate clauses. Vocabulary at A2 level only. The full set of B1 rules above plus tighter.

### CEFR B2

Allows abstract vocabulary and some longer sentences (up to ~25 words). Idiom-free in feedback, but the underlying connectives and phrasing can be more nuanced.

### CEFR C1 or "native"

Skip the language-level rules. Just apply the discipline rules above.

---

## Output structure

```
{Student name in title case}
Overall: {score} / {scale_max} — {band label}

{One short opening sentence. Warm and direct. Address the student in second person.}

WHAT WENT WELL
• {Strength 1 — concrete, with a short quoted example if possible}
• {Strength 2}
• {Strength 3}
(3–4 bullets)

EVEN BETTER IF
• {Action 1 — phrased as a try-this}
• {Action 2}
(Max 2–3 bullets.)

{Optional one closing line — encouraging, forward-looking.}
```

---

## What to cut from the teacher version

- Transcription section — gone.
- Required elements PRESENT/ABSENT table — gone (an absent feature may become one EBI bullet).
- The criterion scores — gone.
- Criterion tags in the Evidence section — pull the quoted content, drop the tag.
- Anything that reads like marking working.

## What to keep

- The overall band, exactly as scored.
- 3–4 genuine strengths from "What's working", rephrased for the target level.
- 2–3 highest-impact issues from "What to work on", rephrased as actions.
- The student's own quoted phrases — verbatim, never simplified.
- The student's name in the opening line.

---

## Pre-save checklist

- [ ] No comparison to other students (including soft phrasings).
- [ ] No rubric jargon.
- [ ] No idioms in feedback prose (if target level is below C1).
- [ ] Sentences within the target-level range.
- [ ] EBI bullets phrased as actions, max 2–3.
- [ ] Student's quoted writing is verbatim.
- [ ] Total under 250 words.
