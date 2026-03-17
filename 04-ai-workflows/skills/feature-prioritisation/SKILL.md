# SKILL: Feature Prioritisation

**Name:** feature-prioritisation
**Version:** 1.1
**Author:** Dushyant Shah
**Description:** Scores a list of features using the RICE or ICE prioritisation framework with AI-assisted rationale. Outputs a scored feature table, top recommendations, and trade-off notes. Designed for PMs who need to bring structure and justification to roadmap conversations.

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
| **Reach** | How many users will this directly affect per quarter? (See B2B note below) | Absolute number |
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

[Sort by RICE score descending. Add a "Priority" label: High (top quartile), Medium, Low, Defer.]

---

## Scoring Rationale

For each feature, provide the reasoning behind the scores:

### [Feature A] — RICE Score: [X]
**Reach:** [How many users directly affected? If B2B, note which role — Admin/Professor/Student — and the downstream unlock.]
**Impact:** [Which NSM or L1/L2 metric does this move? If it doesn't move the NSM tree directly, flag it as a retention or hygiene feature.]
**Confidence:** [What evidence supports this? Where is the uncertainty?]
**Effort:** [Engineering complexity, dependencies, design work needed. For a small team, most features = 1–4 person-weeks.]
**Key assumption:** [The single assumption that, if wrong, would change the score most]

### [Feature B] — RICE Score: [X]
[Same format]

---

## Top Recommendations

Based on the scoring, the top [N] features to prioritise next quarter are:

**1. [Feature Name] (RICE: X)**
[2–3 sentences on why this is #1. What does it unlock? What's the risk of not building it?]

**2. [Feature Name] (RICE: X)**
[Same format]

[Continue for all features if backlog is ≤ 5 items. Cap at top 3 if backlog is larger.]

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
3. Schedule a roadmap review to align stakeholders on the top decisions
```

---

## Generation Instructions

### AI-Estimated Scores

When the user hasn't provided explicit scores, estimate using the product context. State assumptions clearly and be specific about which user role is affected.

**Example for MorphEd (B2B, pilot stage):**
*Scoring "LMS Integration (Moodle / Google Classroom)" — a V2 feature from the roadmap.*

- Reach: 3 pilot institutes × ~10 professors each = 30 professors per quarter. Not all institutes use an LMS — estimate 50% = **15 professors directly affected**.
- Impact: Moves **L2-3 (Unique Professors Generating / Day)** by removing the friction of a separate publishing step for LMS-native institutes. Estimated **1 (medium)** — it increases professor retention but doesn't unlock new assessment supply on its own.
- Confidence: No user research yet on LMS adoption across pilot institutes. Set at **40%** — one stakeholder mention, no validation.
- Effort: Third-party API integration with auth, field mapping, and error handling — estimated **3 person-weeks** for a small team.
- RICE Score: (15 × 1 × 0.40) / 3 = **2.0**

This format — explicit user count, role named, metric named, confidence level justified — is the standard to follow for every feature.

### B2B Reach: Downstream Unlock Rule

In a B2B multi-role product like MorphEd, "Reach" is not simply % of total users. Apply this rule:

- **Admin-facing feature:** Reach = number of Admins directly affected. But note the downstream unlock separately — 1 Admin enabling faster professor onboarding can unlock 10–30 professors. Mention this in the rationale; do not inflate the Reach number itself.
- **Professor-facing feature:** Reach = number of Professors directly affected per quarter. A feature that makes generation faster reaches every active professor.
- **Student-facing feature:** Reach = number of Students directly affected. This is typically the largest absolute number (10–50× more students than professors in any institute), but student features often have lower confidence scores early in the product's life.

At MorphEd's current pilot scale: ~3 institutes, ~10 professors per institute, ~200–500 students per institute. Use these as the base unless the user provides updated figures.

### Impact: Anchor to the NSM Tree

For every feature, map the expected impact to MorphEd's metric framework before assigning an impact score:

- **Does it move L1-1 Supply (Assessments Published / Day)?** → Impact ≥ 1. Features that help professors generate or publish faster belong here.
- **Does it move L1-2 Demand (Completions / Assessment)?** → Impact ≥ 1. Features that increase student start or submit rates belong here.
- **Does it move a specific L2 lever?** → Impact 0.5–2 depending on lever. Identify which lever: Generated/Day (L2-1), Gen→Publish Rate (L2-2), Unique Professors Generating (L2-3), Start Rate (L2-4), Start→Submit Rate (L2-5), Average Score (L2-6).
- **Doesn't move the NSM tree?** → Impact ≤ 0.5 unless it protects a guardrail metric (e.g., hallucination rate ≤ 5%, generation failure rate ≤ 2%). Flag these explicitly as "guardrail / hygiene" features — they may still be important, but they're not NSM drivers.

### Impact Calibration

Use the standard RICE multipliers (0.25, 0.5, 1, 2, 3). Don't assign "10/10 impact" to everything — the distribution should be meaningful. Most features at MorphEd's current stage should score 0.5–1. Reserve 2–3 for features that fundamentally unlock a new segment or close a critical retention gap.

### Confidence Calibration

Be honest about uncertainty.
- Single stakeholder mention or internal assumption → 30–40%
- Multiple professor / admin conversations confirming the pain → 60–70%
- Competitor has shipped it and it demonstrably worked → 70–80%
- Validated with your own users in a test or pilot → 80–90%

MorphEd is at pilot stage. Most impact estimates deserve 40–60% confidence unless backed by direct user evidence.

### Effort Calibration for a Small Team

MorphEd runs on a small engineering team (1–2 engineers). Calibrate effort accordingly:
- Simple UI changes, config updates → 0.5–1 person-week
- New feature with existing patterns (new assessment type using the existing RAG pipeline) → 1–2 person-weeks
- New integration or new technical surface (LMS API, mobile offline sync) → 3–5 person-weeks
- New architecture or platform-level change → 5+ person-weeks

### Trade-Off Notes

This is the most valuable section for stakeholder conversations. The framework produces a number; the trade-off notes produce the judgment. Don't skip this section.

### Deferred Features

Always include a deferred list. A prioritisation output without a deferral list implies that everything is being built — which is never true and undermines the credibility of the output.

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
- For small backlogs (≤ 5 features), rank all features in the Top Recommendations section rather than arbitrarily cutting to top 3
