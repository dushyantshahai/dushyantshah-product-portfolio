# SKILL: PRD Generator

**Name:** prd-generator
**Version:** 1.1
**Author:** Dushyant Shah
**Description:** Generates a structured Product Requirements Document (PRD) from a brief problem statement or feature idea. Designed for PMs who need to go from rough brief to shareable spec quickly.

---

## When to Invoke This Skill

Invoke this skill when the user provides any of the following:
- A rough feature idea or product brief
- A problem statement they want to turn into a spec
- A customer request or pain point they want to formalise
- A one-liner like "I want a PRD for [X]"

Do not invoke this skill if the user is asking for analysis, research, or strategic advice — use the `market-research` or `product-teardown` skill instead.

---

## Behaviour When Invoked

### Step 1: Gather Minimum Context (if not already provided)

Before generating the PRD, ask for the following if not already in the user's message. Ask all questions in one message — do not ask them one by one:

1. **Product / feature name:** What are we building?
2. **User:** Which role(s) does this affect? (For MorphEd: Admin, Professor, Student, or Founder — see role context in Generation Instructions)
3. **Problem:** What specific problem does this solve? What's the user's current workaround?
4. **Context:** Any constraints? (Technical, resource, timeline, token cost sensitivity)
5. **Success:** How will we know it worked? Any initial metric intuition?

If the user has already provided this information, skip Step 1 and proceed directly to generation.

### Step 2: Generate the PRD

Produce a complete PRD in the following structure. Do not skip sections. Do not add sections not in this template.

---

## OUTPUT TEMPLATE

```markdown
# PRD: [Product / Feature Name]

**Author:** [User's name if provided, otherwise leave blank]
**Status:** Draft
**Last Updated:** [Today's date]
**Target Launch:** [If provided, otherwise TBD]
**Roles Affected:** [Admin / Professor / Student / Founder — list all that apply]

---

## 1. Problem Statement

[2–3 sentences. What is the user trying to do? What is broken or missing today? What is the cost of the status quo? Be specific about role, task, and pain — avoid vague language like "users struggle with".]

## 2. Objective

[One sentence. What does this feature achieve for the affected user(s) and for the product's North Star Metric?]

## 3. User Personas

[For each affected role, complete the block below. Use only the roles that apply — do not include roles that have no direct interaction with this feature.]

### [Role: Admin / Professor / Student / Founder]
**Who:** [Brief description of this role's context in MorphEd — see Generation Instructions]
**JTBD:** "When I [situation], I want to [motivation], so that [outcome]."
**Current workaround:** [How they handle this today]
**Pain with current workaround:** [What's frustrating or costly about it]

### [Second role if applicable]
[Same format]

## 4. Success Metrics

| Metric | Type | Target | Measurement Method |
|---|---|---|---|
| [Primary metric] | Leading | [Value] | [How to measure] |
| [Secondary metric] | Lagging | [Value] | [How to measure] |
| [Guardrail metric] | Guardrail | [Value] | [How to measure] |

**NSM Connection:** [How does this feature move "Assessments Completed per Day"? Name the specific L1 or L2 lever it affects. If it doesn't directly move the NSM tree, label it as: Retention / Hygiene / Guardrail Protection.]

## 5. Functional Requirements

[Numbered list of what the product/feature must do. Each requirement must be testable — no vague statements.]

### Must Have
- FR-1: [Requirement]
- FR-2: [Requirement]

### Should Have
- FR-3: [Requirement]

### Could Have (Deferred)
- FR-4: [Requirement]

## 6. Non-Functional Requirements

- **Generation latency:** [If this feature involves LLM calls — e.g., "MCQ generation must complete within 60 seconds of topic selection"]
- **LLM API failure handling:** [Expected behaviour if Claude or Gemini returns an error — graceful message, retry logic, fallback]
- **Token cost impact:** [Does this feature add new LLM calls? Estimate token delta per operation. Flag if material.]
- **Role-based data isolation:** [Confirm no cross-institute data can be exposed. Specify any new access control requirements.]
- **Mobile responsiveness:** [Required for V2 mobile roadmap — specify if this feature needs mobile-first consideration]
- **Performance:** [Non-LLM performance requirements — e.g., page load, API response time]

## 7. User Flow

[Step-by-step primary user journey from trigger to value realised. Use numbered steps. Describe the experience in plain language — no wireframes required.]

1. [Step 1]
2. [Step 2]
...

**Edge cases to handle:**
- [Edge case 1 and expected behaviour]
- [Edge case 2 and expected behaviour]

## 8. Out of Scope

[Explicitly list what this PRD does NOT include. Prevents scope creep.]

- [Out-of-scope item 1]
- [Out-of-scope item 2]

## 9. Open Questions

| # | Question | Owner | Due Date |
|---|---|---|---|
| 1 | [Question] | [Who should answer] | [When needed] |
| 2 | [Question] | [Who] | [When] |

## 10. Dependencies & Risks

**Dependencies:**
- [What this feature depends on — e.g., Claude Haiku API, Gemini Flash API, Supabase schema changes, RAG pipeline, existing content hierarchy]

**Risks:**
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| [Risk 1] | High/Med/Low | High/Med/Low | [How to address] |

## 11. Appendix (Optional)

[Supporting research, competitive context, or design references worth linking]
```

---

## Generation Instructions

### MorphEd Role Context (Pre-Loaded)

When writing a PRD for a MorphEd feature, use this role reference to populate the User Personas section accurately:

| Role | Who they are | Primary job in MorphEd |
|---|---|---|
| **Admin** | Institute administrative coordinator | Manages professor and student accounts, uploads textbooks, creates batches, monitors institute-level analytics. Operates the infrastructure layer — not the teaching layer. |
| **Professor** | Faculty member | Generates MCQ assessments using the RAG pipeline, reviews and edits AI-generated questions, publishes to batches, tracks student and class performance via analytics. |
| **Student** | Enrolled learner | Receives and attempts assessments, submits, views auto-graded results and personal analytics. Passive recipient of what Admin and Professor configure. |
| **Founder** | Product owner / platform operator | Cross-institute visibility — monitors NSM (Assessments Completed per Day), platform health, and key metrics across all institutes. Not a typical daily user of any one feature. |

Most features affect one primary role and one secondary role. Rarely all four. When in doubt: Admin features unlock capabilities for Professors; Professor features create supply for Students; Student features drive the demand side of the NSM.

### Success Metrics — NSM Anchor

Every PRD must include an explicit "NSM Connection" statement. Apply this logic:

- **Does the feature help professors generate or publish more assessments faster?** → Moves L1-1 Supply (Assessments Published / Day). Identify which L2 lever: Generated/Day (L2-1), Gen→Publish Rate (L2-2), or Unique Professors Generating (L2-3).
- **Does the feature help students start or complete more assessments?** → Moves L1-2 Demand (Completions / Assessment). Identify the L2 lever: Start Rate (L2-4), Start→Submit Rate (L2-5), or Average Score (L2-6).
- **Does the feature not directly move the NSM tree?** → Label it explicitly as Retention, Hygiene, or Guardrail Protection. Examples: reducing hallucination rate (Guardrail), improving load time (Hygiene), sending re-engagement notifications (Retention). These are valid features — they just need to be categorised correctly so they don't crowd out NSM-driving work in prioritisation.

Always include at least one leading metric (behaviour-based), one lagging metric (outcome-based), and one guardrail metric (something that would signal harm if the feature degrades it).

### Non-Functional Requirements — MorphEd Defaults

Replace generic boilerplate with MorphEd-relevant defaults. Specifically:

- **Generation latency** is the most user-visible NFR for professor-facing features. The target is < 60 seconds from topic selection to displayed MCQs. Any feature that adds steps to this path must specify its latency budget.
- **Token cost** should be estimated for any feature that introduces a new LLM call. Claude 3 Haiku and Gemini Flash are cost-efficient but at scale (hundreds of assessments/day across institutes) new calls compound quickly. Flag the delta.
- **Role-based data isolation** is non-negotiable. No feature should expose data across institute boundaries. Any new API endpoint or query must be checked against this constraint.
- **WCAG 2.1 AA compliance** is not a default NFR for MorphEd. The product is B2B SaaS sold to Indian institutes with no regulatory accessibility mandate in V1. Include it only if there is a specific reason.
- **Mobile responsiveness** is increasingly relevant — the V2 roadmap includes a native mobile app. Any new UI should be built responsively unless explicitly scoped for desktop-only.

### Functional Requirements — Testability Standard

Write each requirement as a testable statement. Bad: "The interface should be user-friendly." Good: "The system must display a professor's generated MCQs within 60 seconds of submitting a topic selection." Every "Must Have" requirement should have a clear pass/fail condition — if you can't write a test for it, rewrite it until you can.

### Open Questions — MorphEd Recurring Questions

In addition to feature-specific open questions, surface any of the following that are unresolved for the spec in question:

- **Role access:** Are there any roles that should NOT see this feature, or that require a permission gate?
- **RAG pipeline impact:** Does this feature change how content is ingested, chunked, retrieved, or how prompts are constructed? If yes, retrieval quality and LLM output format must be re-validated.
- **Token cost delta:** Does this feature introduce any new LLM calls? If yes, what is the estimated cost per operation and per institute per month?
- **Content hierarchy impact:** Does this feature require changes to the Book → Chapter → Topic → Sub-topic structure, or to how assessments map to content nodes?
- **Data isolation check:** Does any new API endpoint or database query need an institute-scoping filter to prevent cross-institute data exposure?

Not all questions apply to every feature. Only include the ones that are genuinely unresolved — don't pad the Open Questions table with noise.

### Out of Scope — Default Prompt

If the user hasn't specified out-of-scope items, suggest 2–3 that naturally follow from the feature. For MorphEd features, common deferred items include: non-MCQ question types, mobile app version (if desktop-first), multi-language support, LMS integration, and advanced analytics cuts. Use the V1 "Explicitly Out of Scope" list from the PRD as a reference for what was deferred product-wide.

### Problem Statement — Specificity Standard

Avoid vague language. Bad: "Professors struggle with creating assessments." Good: "Professors currently spend 2+ hours per assessment manually writing MCQs from textbooks — searching for relevant content, writing distractors, and checking difficulty balance by hand. MorphEd reduces this to under 5 minutes, but [specific remaining friction point] still requires [manual step], adding [X minutes] back to the workflow."

---

## Tone and Style

- Write in plain, precise English
- Use active voice ("The system generates..." not "MCQs will be generated by the system...")
- Be opinionated where clarity helps: if a requirement is ambiguous, choose a reasonable interpretation and note it as an assumption
- Do not pad the PRD with generic filler — every sentence should carry information
