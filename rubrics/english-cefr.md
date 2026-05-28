# English Holistic Writing Rubric (CEFR-leaning, 1–7)

A holistic rubric for longer pieces of student writing in English (essays, scripts, articles, narrative, opinion). Nine criteria across three dimensions, scored on a 1–7 scale and synthesised into a single overall band.

This rubric is one example of the *rubric file format* used by the writing-marker agent. Users can copy it and adapt for their own subject, scale, or framework. See `rubrics/ib-humanities-essay.md` and `rubrics/generic-argumentation.md` for other examples.

---

## Band scale

| Score | Label |
|---|---|
| 1–2 | Emerging |
| 3 | Developing |
| 4 | Meeting |
| 5–6 | Better than meeting |
| 7 | Exceeding |

One integer per submission. No half-bands. No percentages.

**Note on the inner split.** The descriptors below describe four tiers (1–2, 3–4, 5–6, 7) but the band labels split 3 from 4 and split 5 from 6. When a criterion fits the "3–4" descriptor, pick **3** if the descriptor is barely met and **4** if it is comfortably met. When a criterion fits the "5–6" descriptor, pick **5** if the descriptor is barely met and **6** if it is comfortably met. Apply the same logic to the overall band — do not default to the lower number out of caution.

---

## Criteria

### LANGUAGE

#### Vocabulary
- **7** — Nuanced, purposeful, enhances the message. Includes idiomatic expressions and less common words.
- **5–6** — Generally effective, with some idiomatic expressions and a mix of common and less common words.
- **3–4** — Basic, relying mainly on everyday words. Occasionally overused but generally appropriate.
- **1–2** — Minimal, with difficulty even in basic word choice.

#### Grammatical Structures
- **7** — A variety of simple and complex structures used to enhance communication. Punctuation accurate, verb tenses suit the purpose.
- **5–6** — Both simple and complex sentences, with connectives like *and, but, so*. Tenses mostly appropriate, punctuation generally accurate.
- **3–4** — Mainly simple sentences, limited use of connectives and basic punctuation. Attempts at verb tenses are inconsistent.
- **1–2** — Often run-ons, connectives rarely used, little awareness of verb tenses.

#### Accuracy
- **7** — Almost no errors; grammar, punctuation, spelling carefully monitored.
- **5–6** — Errors occur but do not interfere with communication.
- **3–4** — Errors in complex structures sometimes interfere with meaning.
- **1–2** — Errors are frequent and meaning is often unclear.

> **Spelling note for this rubric.** Orthography is treated as part of *Accuracy*. Do not silently ignore spelling errors when grading. (Other rubrics — e.g. for handwritten subjects where transcription is unreliable, or for spoken-word tasks — may handle this differently. See `rubrics/generic-argumentation.md` for an example that does not weight spelling.)

### MESSAGE

#### Relevance
- **7** — All ideas are relevant.
- **5–6** — Most ideas are relevant.
- **3–4** — Some ideas are relevant.
- **1–2** — Few ideas are relevant.

#### Development
- **7** — Ideas fully developed with details and examples.
- **5–6** — Ideas developed with some detail and examples.
- **3–4** — Ideas outlined but not fully developed.
- **1–2** — Ideas stated without development.

#### Clarity & Organisation
- **7** — Writing is logical, coherent, and responds to the demands of the genre.
- **5–6** — Generally clear and logically structured.
- **3–4** — Structure uneven, clarity inconsistent.
- **1–2** — Lacks logical structure; message difficult to determine.

### CONCEPTUAL UNDERSTANDING

#### Text Type
- **7** — Highly appropriate choice, showing insightful understanding of context, purpose, and audience.
- **5–6** — Appropriate, showing good understanding.
- **3–4** — Generally appropriate but shows only basic understanding.
- **1–2** — Inappropriate.

#### Register & Tone
- **7** — Highly appropriate for the context.
- **5–6** — Appropriate but not always consistent.
- **3–4** — Sometimes appropriate but lacks consistency.
- **1–2** — Inappropriate.

#### Text Type Conventions
- **7** — Conventions fully incorporated.
- **5–6** — Mostly incorporated.
- **3–4** — Some conventions used.
- **1–2** — Few conventions recognisable.

---

## Synthesis — combining the nine into one overall band

The overall band is **holistic, not arithmetic**. Do not average the nine scores. Read them together and produce a defensible whole that reflects the strongest and weakest signals.

**Rules of synthesis:**

1. A serious weakness in any single criterion should weigh the overall down. You cannot ignore a 2 in Accuracy by averaging it away with a 6 in Vocabulary.
2. A criterion that comfortably reaches a higher band can lift the overall if other criteria are at least at the band below. Three 6s and six 4s should land at 5, not 4.
3. The student's lowest criterion is rarely the overall, but it is rarely more than two bands below the overall either. If a piece has Accuracy 2 and everything else at 5, "Better than meeting" overstates it; the right read is usually 4.
4. Use the inner-split rule: when the synthesis sits between bands within a tier (3 vs 4, or 5 vs 6), pick the higher number if the descriptors are *comfortably* met, the lower if *barely*. Don't default downward.

In the per-student marker file, name the criteria that anchored your judgement in one or two sentences.

---

## Grade-ceiling behavior

If the task brief defines **required elements** (specific structural elements that must be present), missing any one of them caps the overall at **Developing (3)** for this rubric. The cap is a *ceiling*, not a fixed band — a piece missing a required element *and* weak in other ways can still land at Emerging (1–2).

When the brief defines no required elements, this section does not apply.

State the cap explicitly in the per-student marker file when it is triggered.

---

## Grade-ceiling band for this rubric

```
grade_ceiling_band: 3
grade_ceiling_label: "Developing"
```

(The orchestrator reads these values to know what "missing a required element" means in this rubric's scale. Other rubrics with different scales set these accordingly — e.g. an IB-style 1–7 might use the same; a 4-point scale might use 2.)

---

## Rubric metadata

```
name: english-cefr
scale: integer
scale_min: 1
scale_max: 7
no_half_bands: true
band_labels:
  1: Emerging
  2: Emerging
  3: Developing
  4: Meeting
  5: Better than meeting
  6: Better than meeting
  7: Exceeding
grade_ceiling_band: 3
spelling_counts_as_accuracy: true
```
