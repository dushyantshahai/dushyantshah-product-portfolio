# Metrics & Success Framework — MorphEd

## North Star Metric

**MCQs Generated per Active Teacher per Week**

This metric captures the core value delivered (MCQ generation), filtered for engaged users (active teachers), over a timeframe that reflects habitual use (weekly).

Why this is the right NSM:
- It moves when teachers are genuinely using the product for assessment creation, not just signing up
- It's sensitive to both product quality (teachers return when MCQs are good) and product scope (more topics → more generation events)
- It directly correlates with the pain point solved: teachers currently spend 45–90 minutes per quiz; each generation event represents time reclaimed

**Target at 6 months:** ≥ 12 MCQs generated per active teacher per week (equivalent to 1 complete quiz per topic, per week)

---

## OKRs — First 6 Months

### Objective 1: Prove that MorphEd creates genuine time value for teachers

| Key Result | Target |
|---|---|
| Average MCQ generation time (upload-to-export) < 5 minutes | ≤ 5 min median |
| % of generated quizzes exported (vs. abandoned) | ≥ 65% |
| Teacher retention at Week 4 (generated at least 1 quiz in week 4) | ≥ 40% |

### Objective 2: Establish a base of habitual teacher users

| Key Result | Target |
|---|---|
| Monthly Active Teachers (generated ≥ 1 quiz in the past 30 days) | 200 MAT by month 6 |
| Week-over-week active teacher growth rate | ≥ 10% MoM |
| Net Promoter Score (NPS) among weekly active teachers | ≥ 40 |

### Objective 3: Validate MCQ quality as a durable product strength

| Key Result | Target |
|---|---|
| % of generated MCQs marked as "acceptable without edits" by teachers | ≥ 70% |
| Average teacher quality rating for generated MCQs | ≥ 4.0 / 5.0 |
| Hallucination rate (questions referencing out-of-syllabus content) | ≤ 3% |

### Objective 4: Build the foundation for institutional monetisation

| Key Result | Target |
|---|---|
| Number of colleges with ≥ 3 active teachers on MorphEd | ≥ 10 colleges |
| Number of institutional pilot conversations initiated | ≥ 5 |
| Teacher-referred teacher signups | ≥ 30% of new teacher registrations |

---

## Funnel Metrics

```
Syllabus Uploaded
      ↓
Topic Hierarchy Reviewed (Activation)
      ↓
First MCQ Batch Generated (Aha Moment)
      ↓
Quiz Exported (Value Realised)
      ↓
Return Visit Within 7 Days (Retention)
      ↓
Second Syllabus Uploaded OR Colleague Referred (Expansion)
```

| Funnel Stage | v1 Baseline | 6-Month Target |
|---|---|---|
| Upload → Hierarchy reviewed | 82% | 85% |
| Hierarchy reviewed → MCQs generated | 74% | 80% |
| MCQs generated → Quiz exported | 61% | 70% |
| Quiz exported → Return within 7 days | 38% | 50% |
| Return user → Referral or second syllabus | 22% | 35% |

---

## Guardrail Metrics

These metrics exist to ensure we don't optimise the NSM at the expense of quality or trust.

| Guardrail | Threshold | Alert Condition |
|---|---|---|
| MCQ hallucination rate | ≤ 3% | > 5% in any weekly cohort |
| Teacher edit rate per MCQ | Monitor for trend | Sudden increase suggests quality drop |
| Generation failure rate (API errors, malformed output) | ≤ 2% | > 3% in any 24-hour window |
| Avg time to first MCQ generation | ≤ 5 min | > 7 min median in any week |
| Support tickets about incorrect questions | Track for trend | > 5 tickets/week |

The hallucination guardrail is the most important. A single teacher using incorrect MCQs in a formal assessment creates reputational damage that's hard to recover from in a trust-sensitive domain like education.

---

## How Success Would Be Measured at Scale

At 10,000+ active teachers (12–18 months), the metrics framework evolves:

**Revenue metrics enter the picture:**
- MRR (Monthly Recurring Revenue)
- Average Revenue per Institution (ARPI) for B2B accounts
- Payback period on CAC by acquisition channel

**Quality metrics become statistical:**
- MCQ quality sampling (random 2% of generated questions reviewed by internal QA)
- A/B testing framework for prompt improvements with power calculations

**Network effects become measurable:**
- Viral coefficient (teachers referred per active teacher per month)
- Campus density metric (% of teachers at a given institution who are active users)

**Retention cohorts replace activation rates:**
- D7, D30, D90 retention by cohort
- "Power user" cohort analysis (top 20% by MCQ volume drive what % of total output?)

---

## Data Infrastructure Required

To track these metrics reliably:

1. **Event tracking:** Mixpanel or Amplitude for funnel analysis and cohort retention
2. **LLM quality logging:** Store all generation inputs/outputs + teacher feedback (thumbs, edits) for quality analysis
3. **Feedback pipeline:** Weekly review of flagged questions to inform prompt updates
4. **Usage dashboard:** Internal Metabase or Retool dashboard for weekly metric review by the founding team
