# SKILL: PRD Generator

**Name:** prd-generator
**Version:** 1.0
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
2. **User:** Who is the primary user? (Role, context, level of technical sophistication)
3. **Problem:** What specific problem does this solve? What's the user's current workaround?
4. **Context:** Any constraints? (Technical, resource, timeline, regulatory)
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

---

## 1. Problem Statement

[2–3 sentences. What is the user trying to do? What is broken or missing today? What is the cost of the status quo?]

## 2. Objective

[One sentence. What does this feature/product achieve for the user and the business?]

## 3. User Personas

### Primary User
**Who:** [Role, context]
**JTBD:** "When I [situation], I want to [motivation], so that [outcome]."
**Current workaround:** [How they solve this today]
**Pain with current workaround:** [What's frustrating about it]

### Secondary User (if applicable)
[Same format as above]

## 4. Success Metrics

| Metric | Type | Target | Measurement Method |
|---|---|---|---|
| [Primary metric] | [Leading/Lagging] | [Value] | [How to measure] |
| [Secondary metric] | [Guardrail] | [Value] | [How to measure] |

**North Star Metric:** [Single metric that best captures value delivered]

## 5. Functional Requirements

[Numbered list of what the product/feature must do. Use "must", "should", "could" to indicate priority (MoSCoW). Be specific — each requirement should be testable.]

### Must Have
- FR-1: [Requirement]
- FR-2: [Requirement]

### Should Have
- FR-3: [Requirement]

### Could Have (Deferred)
- FR-4: [Requirement]

## 6. Non-Functional Requirements

- **Performance:** [e.g., page load < 2s, API response < 500ms]
- **Reliability:** [e.g., 99.9% uptime, graceful degradation]
- **Security:** [e.g., data isolation, auth requirements]
- **Scalability:** [e.g., must handle X concurrent users]
- **Accessibility:** [e.g., WCAG 2.1 AA compliance]

## 7. User Flow

[Step-by-step description of the primary user journey from trigger to value realised. Use numbered steps. No wireframes required — describe the experience in plain language.]

1. [Step 1]
2. [Step 2]
...

**Edge cases to handle:**
- [Edge case 1 and expected behaviour]
- [Edge case 2 and expected behaviour]

## 8. Out of Scope

[Explicitly list what this PRD does NOT include. This is as important as what it does include.]

- [Out-of-scope item 1]
- [Out-of-scope item 2]

## 9. Open Questions

| # | Question | Owner | Due Date |
|---|---|---|---|
| 1 | [Question] | [Who should answer] | [When needed] |
| 2 | [Question] | [Who] | [When] |

## 10. Dependencies & Risks

**Dependencies:**
- [What this feature depends on: other teams, APIs, data, infrastructure]

**Risks:**
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| [Risk 1] | High/Med/Low | High/Med/Low | [How to address] |

## 11. Appendix (Optional)

[Any supporting research, competitive context, or design references worth linking]
```

---

## Generation Instructions

When filling in the template:

- **Problem Statement:** Be specific about the user's pain. Avoid vague language like "users struggle with" — instead write "users currently spend [X time/effort] on [Y task] using [Z method], which results in [negative outcome]."

- **Success Metrics:** Always include at least one leading metric (behaviour-based) and one lagging metric (outcome-based). Include at least one guardrail metric that would indicate harm if the feature degrades it.

- **Functional Requirements:** Write each requirement as a testable statement. Bad: "The interface should be user-friendly." Good: "The system must display extracted topics within 60 seconds of document upload."

- **Out of Scope:** This section prevents scope creep. If the user hasn't specified, suggest 2–3 items that might naturally be requested but aren't in the spec.

- **Open Questions:** Generate real open questions based on ambiguities in the brief. Don't ask trivial questions; ask the questions a PM would raise in a spec review meeting.

---

## Tone and Style

- Write in plain, precise English
- Use active voice ("The system generates..." not "MCQs will be generated by the system...")
- Be opinionated where clarity helps: if a requirement is ambiguous, choose a reasonable interpretation and note it as an assumption
- Do not pad the PRD with generic filler — every sentence should carry information
