# SKILL: Feature Prioritisation

**Name:** feature-prioritisation
**Version:** 1.0
**Author:** Dushyant Shah
**Description:** Scores a list of features using the RICE or ICE prioritisation framework with AI-assisted rationale. Outputs a scored feature table, top 3 recommendations, and trade-off notes. Designed for PMs who need to bring structure and justification to roadmap conversations.

---

## When to Invoke This Skill

Invoke this skill when the user:
- Provides a list of features, ideas, or initiatives to prioritise
- Asks for help ranking or scoring a backlog
- Wants to build a data-supported case for a roadmap decision
- Says anything like "help me prioritise these", "which feature should we build first", or "score my backlog"

Do not invoke this skill for a single feature decision — use the `prd-generator` skill to spec it out instead.

---

## Behaviour When Invoked

### Step 1: Gather Input (if not already provided)

Ask in a single message:

1. **Feature list:** Provide the list of features or initiatives to prioritise (as many as needed — there's no limit).
2. **Framework preference:** RICE (Reach × Impact × Confidence / Effort) or ICE (Impact × Confidence × Ease)? If not specified, default to RICE for product roadmaps and ICE for experiment prioritisation.
3. **Context:** What product/team is this for? What's the current goal or OKR this roadmap should serve?
4. **Scoring guidance:** Should I ask scoring questions for each feature, or do you want me to apply reasonable AI estimates based on the context you've provided?

If the user has provided the feature list and enough context, default to AI-estimated scoring (note it clearly) and generate the output.

### Step 2: Score and Generate Output

Apply the chosen framework and produce the output in the following structure:

---

## OUTPUT TEMPLATE

```markdown
# Feature Prioritisation: [Product / Initiative Name]

**Framework:** [RICE / ICE]
**Goal / OKR this serves:** [What strategic objective this roadmap supports]
**Date:** [Today's date]
**Scoring method:** [AI-estimated / User-provided inputs / Collaborative]

---

## Scoring Criteria

### RICE Framework
| Parameter | Definition | Scale |
|---|---|---|
| **Reach** | How many users will this affect per quarter? | Absolute number or relative (1–10) |
| **Impact** | How much will this move the target metric per affected user? | 0.25 = minimal, 0.5 = low, 1 = medium, 2 = high, 3 = massive |
| **Confidence** | How confident are we in the Reach and Impact estimates? | % (100% = certain, 50% = uncertain) |
| **Effort** | How many person-weeks to design + build + ship? | Person-weeks |

**RICE Score = (Reach × Impact × Confidence) / Effort**

### ICE Framework (alternative)
| Parameter | Definition | Scale |
|---|---|---|
| **Impact** | Expected impact on target metric | 1–10 |
| **Confidence** | Confidence in the estimate | 1–10 |
| **Ease** | How easy is this to build? (10 = very easy) | 1–10 |

**ICE Score = Impact × Confidence × Ease / 10** (normalised)

---

## Scored Feature Table

| # | Feature | Reach | Impact | Confidence | Effort | RICE Score | Priority |
|---|---|---|---|---|---|---|---|
| 1 | [Feature A] | | | | | | High |
| 2 | [Feature B] | | | | | | Medium |
| 3 | [Feature C] | | | | | | Low |
...

[Sort by RICE score descending. Add a "Priority" label: High (top quartile), Medium, Low, Defer.]

---

## Scoring Rationale

For each feature, provide the reasoning behind the scores:

### [Feature A] — RICE Score: [X]
**Reach:** [Why this reach estimate? What % of users does this touch?]
**Impact:** [Why this impact level? What metric does it move and by how much?]
**Confidence:** [What evidence supports this? Where is the uncertainty?]
**Effort:** [Engineering complexity, dependencies, design work needed]
**Key assumption:** [The single assumption that, if wrong, would change the score most]

### [Feature B] — RICE Score: [X]
[Same format]

---

## Top 3 Recommendations

Based on the scoring, the top 3 features to prioritise next quarter are:

**1. [Feature Name] (RICE: X)**
[2–3 sentences on why this is #1. What does it unlock? What's the risk of not building it?]

**2. [Feature Name] (RICE: X)**
[Same format]

**3. [Feature Name] (RICE: X)**
[Same format]

---

## Trade-Off Notes

[Identify 2–3 meaningful tensions in the prioritisation — features that scored high on RICE but have qualitative reasons to deprioritise, or features that scored lower but have strategic value the framework doesn't fully capture.]

### Trade-off 1: [Feature X] vs. [Feature Y]
[The RICE score says X, but here's the argument for Y: ...]

### Trade-off 2: High-Impact, Low-Confidence Features
[Features with high potential but uncertain estimates — flagged for validation before building]

---

## Deferred Features

| Feature | RICE Score | Reason for Deferral |
|---|---|---|
| [Feature] | [Score] | [Low score / wrong stage / dependency blocker / etc.] |

---

## Suggested Next Steps

1. Validate the top [1–2] assumptions that could change the ranking most
2. Spec the #1 priority feature using the `prd-generator` skill
3. Schedule a roadmap review to align stakeholders on the top 3 decisions
```

---

## Generation Instructions

- **AI-estimated scores:** When the user hasn't provided explicit scores, use the product context to estimate. State the assumption clearly: "Assuming MAU of ~5,000 teachers, Reach for the Google Form Export feature is ~2,500 teachers/quarter (50% of active users with this use case)."

- **Impact calibration:** Use the standard RICE multipliers (0.25, 0.5, 1, 2, 3). Don't assign "10/10 impact" to everything — the distribution of impact scores should be meaningful. Most features should score 0.5–1. Reserve 2–3 for genuinely high-impact bets.

- **Confidence:** Be honest about uncertainty. A feature based on a single user interview deserves 30–40% confidence. A feature based on a survey of 100 users and a competitor who shipped it successfully deserves 70–80%.

- **Trade-Off Notes:** This is the most valuable section for stakeholder conversations. The framework produces a number; the trade-off notes produce the judgment. Don't skip this section.

- **Deferred Features:** Always include a deferred list. A prioritisation output without a deferral list implies that everything is being built — which is never true and undermines the credibility of the output.

---

## RICE vs. ICE Guidance

**Use RICE when:**
- Scoring a product roadmap or quarterly planning backlog
- Comparing features with very different scales of impact
- You have (or can estimate) team effort data

**Use ICE when:**
- Prioritising experiments or growth tests
- Moving fast and don't want to estimate reach separately from impact
- The team doesn't have reliable effort estimates yet

**Default:** RICE for roadmaps, ICE for experiment queues. If the user doesn't specify, default to RICE for product features.

---

## Tone and Style

- Be direct and opinionated — the value of this skill is in the judgment, not just the arithmetic
- Flag assumptions explicitly — every score rests on assumptions; make them visible
- Keep rationale concise — 2–4 sentences per feature, not paragraphs
- Don't hide behind the framework — the trade-off notes should demonstrate PM judgment beyond what the numbers show
