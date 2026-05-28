---
name: synthesizing-batch-issues
description: Read all per-student marker files for a batch and produce a teaching-priorities document. Pattern-mines recurring issues with student counts and quoted examples, surfaces strengths to protect, and outputs a ranked punch-list for the next 2–3 lessons. Use after marking (and optionally calibration) to give the teacher an actionable across-the-class view.
---

# Synthesizing Batch Issues

You are turning a stack of per-student marker files into a single teaching-priorities document. The marker decided what's true for each student; you decide what's true for the *cohort*. A teacher reading your output should be able to plan the next 2–3 lessons from it.

## When to invoke

- After all per-student marker files are written.
- Best invoked after `calibrating-a-batch` so the input marks are internally consistent.
- Once per batch.

## Input contract

- All per-student marker files in a batch.
- The task brief.
- The rubric file (so you know what each criterion was meant to measure).

## Output contract

- A single plain-text document at `{batch}-synthesis.txt`. No markdown syntax — teachers read this in Notes, Word, or email. Use ALL CAPS section headers and `-` bullets.
- Length: 800–1500 words for typical batches; 400–800 words for batches under 10 students.

---

## Core discipline rules

### 1. Count, don't impressionistically claim

Every pattern you name comes with a count. "Subject-verb agreement issues in 14/27 students" is useful; "many students struggle with subject-verb agreement" is not. The teacher needs to know whether to plan a whole-class lesson or a small-group intervention.

**The count threshold depends on batch size:**

- Batch size ≥ 10: include patterns appearing in **4+ students**.
- Batch size 6–9: include patterns appearing in **3+ students**.
- Batch size < 6: include patterns appearing in **2+ students**, and the synthesis should explicitly note "small batch — patterns shown are indicative, not statistical".

Patterns below the threshold go in a brief "less common but worth knowing" mention if at all. The exception: if a single instance is high-impact (e.g. a student who wrote something brilliant), name it.

### 2. Quote real student writing

Every pattern is illustrated with 2–3 short verbatim quotes from the marker files. Quotes make the pattern concrete and help the teacher recognise it when they next see it in the wild. Do not paraphrase — pull the exact wording from the marker files.

### 3. Group by category, not by student

The output is organised by *issue*, not by student. A teacher should be able to look up "comma splices" and see which students it affects; they should not have to read 27 student files to discover that.

### 4. Surface strengths to protect

A teaching-priorities document that only lists errors implicitly tells the teacher their cohort is failing. Include a section on what many students *did well*. The teacher needs to know what to protect when responding to the issues — don't accidentally drive out a strength while fixing a weakness.

### 5. End with a ranked punch-list

The document's most useful single piece is the bottom: a numbered list of 3–5 concrete teaching priorities for the next 2–3 lessons. Highest leverage first. Each item names the issue, the students it affects, and the kind of activity that would address it.

---

## Categories to look for

These are the standard buckets. Not every batch will populate every category — only include sections that have signal.

### A. Required-element gaps

If the task brief defined required structural elements, which were most often missing or weak? How many students were capped? Which features were uniformly present? This is usually the most actionable section.

### B. Recurring language / accuracy patterns

The grammar, spelling, punctuation, and surface-level issues. Group by type:

- Subject-verb agreement (and what kind of subjects trigger it — proper nouns, collective nouns, abstract nouns).
- Tense slippage (past → present mid-paragraph).
- Run-on sentences and comma splices.
- Articles and countability.
- Verb forms after modals and *to*.
- Spelling — and specifically *which high-frequency words* are misspelled repeatedly across the cohort (this becomes a spelling sprint list).
- L1 calques (constructions transferred from the students' first language).
- Question word order.
- Pronoun case.

### C. Genre / form convention issues

Issues specific to the task type. For scripts: slugline format, action lines vs description, parentheticals vs speaker cues. For opinion articles: hook strength, counter/response merging, two reasons that restate each other, weak conclusions, register slips toward speech. For narrative: scene-setting, dialogue tagging. For academic essay: thesis placement, paragraph topic sentences.

### D. Argumentation or storytelling issues (depending on genre)

Higher-level moves: how convincingly does the cohort argue, develop a story, structure a counterargument? This section is often shorter but identifies the harder teaching targets.

### E. Bonus-feature uptake

If the brief named bonus features (rhetorical questions, varied openers, shot direction, etc.), how many students attempted them, and how many used them purposefully? Bonus-feature uptake correlates with the higher bands and is worth naming so the teacher can name-and-credit it publicly.

### F. Strengths observed broadly

What the cohort consistently did well. Include 3–5 strengths with student counts. Avoid the temptation to undercount strengths — a teacher who sees a long list of issues and one line of "strengths" reads that as a failing cohort.

---

## What NOT to do

### Do not re-mark

If you find yourself disagreeing with a marker file's score, do not adjust it. That is the calibration skill's job, not this one. You take the marker files as authoritative input.

### Do not include named-student rankings

The synthesis is about patterns, not individuals. Naming the top three students or the bottom three turns the document into a leaderboard. If you need to name a student to illustrate a pattern, do — but never as part of a ranking.

### Do not invent a punch-list disconnected from the data

The teaching priorities at the end follow from the patterns above. If you found no comma-splice issues, do not recommend a comma-splice lesson because it sounded sensible. Every recommendation traces back to a counted pattern.

### Do not pad

Aim for 800–1500 words. If the batch has only one significant pattern, write a shorter document. If you find yourself padding, stop and shorten.

---

## Output format (plain text, `.txt`)

```
WRITING ISSUES — {batch name}

Batch: {N} students
Task: {one-line task description, e.g. "Opinion article on school uniforms"}
Rubric: {rubric file name}


A. REQUIRED-FEATURE GAPS

{Per-feature: present in N/total. Quote examples. Note grade-ceilings.}


B. RECURRING LANGUAGE / ACCURACY PATTERNS

Subject-verb agreement (N/total)
- "{quoted error}" (Student name if relevant, optional)
- "{quoted error}"
{One sentence on the pattern shape.}

{Next pattern…}


C. GENRE / FORM CONVENTION ISSUES

{Sub-headed by convention.}


D. {ARGUMENTATION / STORYTELLING / etc, depending on genre}

{...}


E. BONUS-FEATURE UPTAKE

{Per bonus feature: N/total used it, M/total used it purposefully. Quote one or two.}


F. STRENGTHS OBSERVED BROADLY

- {Strength}: N/total
- {Strength}: N/total


TEACHING PUNCH-LIST — NEXT 2–3 LESSONS

1. {Highest-leverage lesson}. Affects {N} students. Concrete activity: ...
2. {Next lesson}. Affects {N} students. ...
3. {Next}. ...


QUICK NUMBERS

Pattern                                | N / {total}
---------------------------------------+------------
...                                    | ...
```

---

## Worked example (excerpt — single section)

```
### Subject-verb agreement (14/27)

Especially with collective and abstract subjects: *young people, everyone, students, AI*.

- "young people doesn't always listen" (Angela)
- "every one have different talent" (Isabella)
- "AI need internet" (Soulmate)
- "both ideas is really good" (Asher)

The pattern is that students treat plural-looking collective nouns as if the noun's apparent meaning controls the verb, rather than its grammatical number. *Young people are* feels right because "people" is plural; *young people has* sometimes appears because it's being read as a single concept.

Teaching response: a focused 15-minute mini-lesson on plural-but-singular-looking subjects, with practice sentences that swap between *young people are / young people have* and *everyone is / everyone has*.
```
