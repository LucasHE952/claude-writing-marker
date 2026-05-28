---
name: reading-handwritten-work
description: Transcribe handwritten student work from one or more photos into a verbatim text block, with multi-page handling, mismatched-student detection, and an illegibility discipline. Use this skill before any scoring step when the input is image files.
---

# Reading Handwritten Work

You are reading handwritten student writing from photos. You are the OCR layer — there is no transcription step before you. Your job is to produce a faithful, defensible transcription so that scoring decisions downstream are based on what the student actually wrote.

## When to invoke

- The student work is supplied as image files (`.jpg`, `.png`, `.pdf`).
- Multi-page work is signalled by a numeric suffix in filenames (`alex_1.jpg`, `alex_2.jpg`) or by being grouped in a per-student folder.

Skip this skill entirely if the input is already text (`.txt`, `.md`, `.docx`).

## Input contract

- One or more image files for a single student.
- The task brief (so you know what to skip — printed rubrics, prompts, instructions on the first page).
- Optionally: the student's name as labelled by the teacher, for cross-page verification.

## Output contract

- A **transcription block** in plain text, verbatim, with `[illegible]` markers where appropriate.
- A **transcription confidence note** flagging any of the issues below.

If you cannot produce a defensible transcription (severe image quality, mismatched pages, etc.), do not invent one — stop and surface the issue.

---

## Core discipline rules

### 1. Verbatim or `[illegible]` — nothing in between

Transcribe exactly what you see. If a word is unreadable, write `[illegible]`. If a span of words is unreadable, write `[illegible: ~3 words]`. Never guess, never silently smooth, never correct the student's spelling or grammar in the transcription. The student's errors are *part of the data* the marker downstream will work with.

Owned-by-you OCR errors corrupt every downstream decision — the prior batch that motivated this skill had "new" misread as "now" and then graded as a student error. You own the read; own the consequences of misreading.

### 2. Handwriting messiness is not a basis for deduction

A piece written in messy handwriting that you can still read is just a piece written in messy handwriting. Penmanship is not a writing skill being assessed in this rubric (unless the rubric explicitly says so). Do not flag "handwriting was poor" as evidence of weak writing. The medium is not the message.

**One exception:** if the work is so unreadable that whole ideas cannot be retrieved — three or more consecutive `[illegible]` spans, or a whole sentence you cannot reconstruct — then *Clarity & Organisation* legitimately suffers. The cost is to Clarity (the writing failed to communicate), not to the student's penmanship. Frame any downstream criticism as a Clarity issue, not a handwriting issue.

### 3. Be especially careful with messy sections

In sections that are dense, smudged, or hurried, slow down. Re-read. Look for the word elsewhere in the piece — students often write the same word multiple times, and one instance may be legible even when another isn't. If after careful reading the word is still uncertain:

- If you have a confidence above ~80%, transcribe it and note the uncertainty in the confidence note.
- If below, mark `[illegible: looks like "X" or "Y"]` and let the marker downstream see both candidates.
- Never silently pick one. The discipline is to make uncertainty visible.

---

## Multi-page handling

### Page-order verification

Filenames are a hint, not authority. After reading page N, ask: *does page N+1 start where page N ended?* If page 1 ends mid-sentence and page 2 starts a completely new scene or paragraph, the page order is suspect. Flag in the confidence note before continuing.

Common signals of correct order:
- Sentence runs across the page break.
- Numbered scene/section continues.
- Narrative time advances naturally.

Common signals of wrong order:
- Page N ends on a full stop with a clear conclusion; page N+1 begins mid-thought.
- Topic abruptly changes between pages.
- Page numbers written by the student themselves contradict the filename order.

### Missing page detection

If page 1 ends mid-sentence and page 2 begins with a new paragraph that does not continue that sentence, a page is probably missing between them. Flag this rather than treat the gap as a student error.

### Mismatched student detection

Same name across pages does not guarantee same student. Some classes have multiple students with the same first name (and a teacher labelling the photos may not have noticed). Compare handwriting style — slant, letter formation, spacing, ink colour — across pages.

If the handwriting style breaks sharply between pages bearing the same student's name, *do not merge them*. Flag in the confidence note: "Pages 1–2 and page 3 appear to be different students despite shared label". The marker downstream can either ask the teacher or treat them as separate pieces.

### Skip non-student content

The first page often contains material the teacher did not crop out: a printed rubric, the task prompt, instructions for the student, a school logo. Identify and skip these. The student's writing usually starts:
- Below or after the rubric block.
- Where the handwriting changes from printed-and-distributed text to handwritten student writing.
- After a clear visual break.

Note in the confidence note when you have skipped non-student content (e.g. "Skipped printed rubric at top of page 1").

### Cross-page sentence reassembly

A sentence that breaks across pages must be reassembled in the transcription, not transcribed as two broken fragments. If page 1 ends `"...I went to the"` and page 2 begins `"market to buy bread."`, the transcription reads `"...I went to the market to buy bread."` — with the page boundary marker placed where it falls within the reassembled text.

Use a marker like `[page 2]` or `--- page 2 ---` to preserve the structural information for the marker downstream, but never break a sentence visually.

### Honour explicit student navigation

Students sometimes draw arrows, write "→ continued on page 4", number insertions, or mark an inserted paragraph in the margin. The student's intended reading order takes precedence over the physical page order. Follow their navigation when transcribing.

### Crossings-out are visible self-edits

When a student crossed something out, the visible edit is information. Transcribe as:

```
[crossed out: "I am going to lost"] forget my homework
```

Then in scoring, the marker downstream credits the revising behaviour (positive evidence of drafting) but grades only the final, uncrossed version. Don't grade the crossed-out content.

---

## Image-quality stop conditions

If any of the following apply, do not guess — surface the issue in the confidence note and ask whether to proceed:

- The page is photographed sideways or upside down and rotating it does not produce clean reading orientation.
- Resolution is so low that individual letters cannot be distinguished.
- Strong shadow or lighting issues obscure more than ~25% of the writing.
- The page is partly out of frame (text running off the edge).

Better to ask for a rephotograph of one page than to silently produce a low-quality transcription that distorts the mark.

---

## Output format

```
## Transcription

[skipped: printed rubric at top of page 1]

{verbatim student writing, with [illegible] markers, [crossed out: "..."] for revisions, and [page N] for page boundaries}

## Transcription confidence note

- Pages read in order: {1, 2, 3}.
- Pages skipped: {none, or list with reason}.
- Illegible spans: {none, or list with locations}.
- Mismatched-student suspicion: {none, or details}.
- Image-quality issues: {none, or details}.
- Other notes: {none, or details}.
```

If everything is clean, the confidence note can be one line: "All pages read in order. No illegible spans. No anomalies."

---

## Worked example

**Input:** two photos, `priya_1.jpg` and `priya_2.jpg`. Page 1 has a printed rubric at the top, then the student's handwritten article starts halfway down. Page 1 ends mid-sentence ("I think uniform is..."). Page 2 begins "good for school because..." in the same handwriting.

**Output:**

```
## Transcription

[skipped: printed rubric at top of page 1]

Priya

Should schools have uniform? I think uniform is good for school because everyone look same and you save time in morning. My friend [illegible] she always wake up [crossed out: "ealy"] early to choose her clothes...

## Transcription confidence note

- Pages read in order: 1, 2.
- Skipped printed rubric at top of page 1.
- One illegible word in paragraph 1 (a friend's name, ~5 letters, "Mar..." or "Mer...").
- Otherwise clean.
```
