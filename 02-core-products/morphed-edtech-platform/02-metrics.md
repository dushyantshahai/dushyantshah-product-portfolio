 * Repo: Dushyant Shah Product Portfolio
 * Author: Dushyant Shah
 * GitHub: https://github.com/dushyantshahai
 * This code is part of a public portfolio and is not licensed for reuse.
 * © 2026 Dushyant Shah
---
# Metrics & Success Framework — MorphEd

## North Star Metric

**Assessments Completed per Day**

This metric captures the ultimate value exchange on the platform — every completion represents a student actively learning and a professor successfully gathering assessment data. It is the single number that moves when both supply (professors generating and publishing) and demand (students attempting and submitting) are healthy.

Why this is the right NSM:
- It only moves when the full loop closes — generation, publishing, and student completion
- It is equally sensitive to supply-side failures (professors not generating or publishing) and demand-side failures (students not attempting or completing)
- It directly reflects MorphEd's core value proposition: assessments that are good enough to be used, and engaging enough to be completed

---

## NSM Metric Tree

```
                    ┌─────────────────────────────────┐
                    │         NSM (Level 0)            │
                    │   Assessments Completed / Day    │
                    └────────────┬────────────────────-┘
                                 │
               ┌─────────────────┴──────────────────┐
               │                                    │
   ┌───────────▼────────────┐          ┌────────────▼───────────┐
   │     L1-1: Supply        │          │     L1-2: Demand        │
   │  Assessments Published  │          │   Avg. Completions /    │
   │         / Day           │          │      Assessment         │
   └───────────┬─────────────┘          └────────────┬───────────┘
               │                                     │
    ┌──────────┼──────────┐               ┌──────────┼──────────┐
    │          │          │               │          │          │
┌───▼───┐ ┌───▼───┐ ┌────▼────┐     ┌───▼───┐ ┌───▼───┐ ┌────▼────┐
│ L2-1  │ │ L2-2  │ │  L2-3   │     │ L2-4  │ │ L2-5  │ │  L2-6   │
│ Asses-│ │  Gen  │ │ Unique  │     │ Start │ │ Start │ │  Avg.   │
│ ments │ │  →    │ │  Profs  │     │ Rate  │ │  →    │ │ Student │
│ Gen / │ │ Pub   │ │ Generat-│     │       │ │Submit │ │  Score  │
│  Day  │ │ Rate  │ │ ing/Day │     │       │ │ Rate  │ │         │
└───────┘ └───────┘ └─────────┘     └───────┘ └───────┘ └─────────┘
```

### Level 1 — Input Metrics

- **L1-1 Supply (Published / Day):** Measures how much fresh content is being added to the platform. More published assessments = more opportunities for students to participate.
- **L1-2 Demand (Completions / Assessment):** Measures the pull of each assessment. A published assessment with zero completions means the supply is ineffective.

### Level 2 — Product Levers

**Supply side:**
- **L2-1 Generated / Day:** Raw output of the RAG engine — measures AI efficiency and professor activity
- **L2-2 Generation → Publish Rate:** How much of the AI output meets professor standards without requiring re-generation
- **L2-3 Unique Professors Generating / Day:** Growth of the content creator base

**Demand side:**
- **L2-4 Assessment Start Rate:** Measures the hook of the assessment — title, subject relevance, student motivation
- **L2-5 Start → Submit Rate:** Measures friction in the assessment-taking interface
- **L2-6 Average Student Score:** Indirect signal of assessment difficulty calibration and content quality

---

## OKRs — First 6 Months

### Objective 1: Prove that MorphEd creates genuine time value for professors

| Key Result | Target |
|---|---|
| Average book upload & TOC creation time | ≤ 5 min median |
| Average MCQ generation time (book selection → ready questions & options) | < 2 min median |
| % of generated assessments published by professor | ≥ 80% |
| Teacher retention at Week 4 (generated ≥ 2 assessments in 4 weeks) | ≥ 50% |

### Objective 2: Establish a base of habitual professor users

| Key Result | Target |
|---|---|
| Monthly Active Teachers (generated ≥ 3 assessments in 4 weeks) | 50 MAT by end of month 3 |
| Week-over-week active teacher growth rate | ≥ 25% WoW* |
| Net Promoter Score (NPS) among active teachers | ≥ 70 |

*Derived from the 50 MAT target: reaching 50 MAT by week 13 from a cold start (~2 active teachers at week 1) requires a WoW growth rate of approximately 28%. Target set at ≥ 25% to account for early ramp-up variability.

### Objective 3: Validate MCQ quality as a durable product strength

| Key Result | Target |
|---|---|
| % of generated assessments published after 1 generation (no re-generation required) | ≥ 70% |
| Average teacher quality rating for generated MCQs | ≥ 3.0 / 5.0 |
| Hallucination rate (questions referencing out-of-syllabus content) | ≤ 5% |

---

## Funnel Metrics

The MorphEd platform has two user funnels that must both be healthy for the NSM to grow — the **professor funnel** (supply) and the **student funnel** (demand).

### Professor Funnel (Supply)

```
Book Uploaded by Admin
        ↓
TOC Generated & Hierarchy Reviewed  ← Activation
        ↓
First Assessment Generated           ← Aha Moment
        ↓
Assessment Published to Batch        ← Value Realised
        ↓
Return Generation Within 7 Days      ← Retention
        ↓
≥ 3 Assessments Generated in 4 Weeks ← Habitual Use (MAT definition)
```

### Student Funnel (Demand)

```
Assessment Published (available to student)
        ↓
Assessment Started                   ← Activation
        ↓
Assessment Submitted                 ← Value Realised (NSM event)
        ↓
Results Viewed                       ← Feedback loop
        ↓
Next Assessment Attempted            ← Retention
```

---

## Guardrail Metrics

These metrics exist to ensure we don't optimise the NSM at the expense of quality or trust.

| Guardrail | Threshold | Alert Condition |
|---|---|---|
| MCQ hallucination rate | ≤ 5% | > 5% in any weekly cohort |
| Teacher edit rate per MCQ | Monitor for trend → arrive at benchmark | Sudden spike suggests quality drop |
| Generation failure rate (API errors, malformed output) | ≤ 2% | > 3% in any 24-hour window |

The hallucination guardrail is the most important. A single professor using incorrect MCQs in a formal assessment creates reputational damage that is hard to recover from in a trust-sensitive domain like education. The teacher edit rate is currently being tracked to establish a baseline — once a trend is visible, a benchmark threshold will be set and added here.
