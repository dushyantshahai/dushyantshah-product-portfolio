# Decision Log 003: MCQ-Only in V1

**Date:** January 2026
**Decision area:** Scope / product strategy
**Status:** Outcome confirmed ✅

---

## Context

Assessment formats span a wide range: multiple choice, short answer, fill-in-the-blank, match the pairs, essay, and case-based questions. All of them exist in Indian academic institutions. The product design question was: **how many question types should MorphEd support at launch?**

The temptation to support multiple types from day one was real — broader support would make the product feel more complete and reduce the "but we also need X" objection in sales conversations. The counter-argument was focus and signal quality.

---

## Options Considered

**Option A: Multi-format from launch (MCQ + short answer + fill-in-the-blank)**
Support the three most common formats used in Indian institutions from V1.
- Broader appeal; fewer objections during pilots
- Subjective formats (short answer, essay) require either human grading or a complex NLP evaluation layer — neither is ready for V1
- Without auto-grading on subjective types, the analytics loop breaks: you can generate the questions but can't close the feedback loop with structured performance data
- Each format requires its own generation prompt, review UI, and export format — 3x the build surface
- Quality control becomes harder: it's easier to validate MCQ generation quality than to validate open-ended question quality

**Option B: MCQ-only in V1, expand in V2**
Ship with MCQ exclusively. Build the full loop — generation, delivery, auto-grading, analytics — for one format and validate it completely before expanding.
- MCQs are deterministic: one correct answer, instantly auto-gradable, no subjective evaluation required
- Structured output means every wrong answer maps to a specific topic, chapter, and difficulty level — feeding directly into analytics
- Reduces build scope significantly, allowing faster time-to-demo
- The "only MCQs" constraint forces quality — every part of the stack is optimised for one format

---

## Choice

**Option B — MCQ-only in V1.**

---

## Rationale

The decisive factor was the analytics feedback loop. MCQs produce structured, deterministic data: correct or incorrect, mapped to a specific topic and difficulty. This data feeds directly into the professor and student analytics dashboards. A wrong answer isn't just a wrong answer — it's a signal about which topic needs more coverage.

Subjective formats break this loop. Without deterministic grading, you can generate questions but can't close the measurement cycle. For a V1 whose goal was to prove that assessments could be generated, delivered, and measured end-to-end, MCQs gave the cleanest possible test.

The "MCQ is 80% of teacher demand" observation from user interviews reinforced the choice. Most internal assessments at coaching institutes and junior colleges are MCQ-dominant — it's the format professors default to because it's the fastest to mark and easiest to standardise.

---

## Outcome / Reflection

The MCQ-only constraint proved correct. The analytics layer is clean, auto-grading is 100% accurate (deterministic), and the generation quality is high enough that professors typically publish after reviewing without needing full regeneration.

The objection — "we also need short answer questions" — has come up in conversations but has not blocked any institutional pilots. The V2 roadmap includes subjective formats, but the unlock condition is right: only after the MCQ infrastructure is solid and the auto-grading accuracy baseline is established.
