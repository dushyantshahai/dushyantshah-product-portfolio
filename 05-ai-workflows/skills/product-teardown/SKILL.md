# SKILL: Product Teardown

**Name:** product-teardown
**Version:** 1.0
**Author:** Dushyant Shah
**Description:** Runs a structured PM-style teardown of any product. Outputs UX analysis, growth loops, monetisation model, competitive moat, and actionable PM takeaways. Designed to produce the quality of thinking a senior PM would bring to a product review — analytical, opinionated, and grounded in product principles.

---

## When to Invoke This Skill

Invoke this skill when the user:
- Names a specific product they want to analyse ("tear down Notion", "analyse Duolingo")
- Asks for a deep-dive on how a product works, retains users, or makes money
- Wants to learn from a competitor's product design
- Is preparing for a PM interview case study on a specific product
- Says anything like "how does [X] work", "why is [Y] successful", or "what can I learn from [Z]"

---

## Behaviour When Invoked

### Step 1: Confirm the Product and Focus (if not clear)

Ask in a single message:

1. **Product name:** Which product are we tearing down?
2. **Focus areas:** Any specific aspects to prioritise? (e.g., onboarding, retention, monetisation, AI features, growth loops). If not specified, run the full standard teardown.
3. **Lens:** Any particular context for this teardown? (e.g., "I'm building a competitor", "preparing for a PM interview", "learning from this product for my EdTech startup")

If the product is named and no specific focus is requested, proceed with the full teardown template.

### Step 2: Generate the Teardown

Produce the teardown in the following structure:

---

## OUTPUT TEMPLATE

```markdown
# Product Teardown: [Product Name]

**Teardown by:** Dushyant Shah
**Date:** [Today's date]
**Focus:** [Full teardown / specific focus areas]

---

## 1. Product Overview (60-second context)

**What it is:** [One sentence description]
**Core value proposition:** [What does the user get from this product that they can't easily get elsewhere?]
**Business model:** [How it makes money — freemium, subscription, marketplace, advertising, etc.]
**Scale signals:** [User base, revenue, growth rate — public data only. Note if estimated.]

---

## 2. Core UX Analysis

### First-Time User Experience
[Describe the onboarding flow from the perspective of someone using the product for the first time. What is the first screen? What actions are required before value is delivered? Where is the aha moment, and how quickly does the user reach it?]

### Core Interaction Loop
[What does the user do repeatedly in this product? Describe the atomic unit of value — the single action + feedback cycle that represents the product's core experience.]

### UX Strengths
- [Specific UX decision that works well and why]
- [Another strength]

### UX Weaknesses / Friction Points
- [Where the UX creates unnecessary friction]
- [A design decision that may be limiting adoption or retention]

---

## 3. Growth Loops

[Identify 2–4 growth loops that drive acquisition, activation, retention, or referral. For each loop, describe: trigger → action → reward → re-entry.]

### Loop 1 — [Loop Name]
**Type:** [Acquisition / Retention / Viral / Paid]
**Mechanism:** [How this loop works]
**Why it's effective:** [The psychological or structural reason this loop creates compounding growth]

### Loop 2 — [Loop Name]
[Same format]

---

## 4. Monetisation Model

**Model type:** [Freemium / Subscription / Marketplace / Usage-based / Advertising / etc.]

**Freemium breakdown (if applicable):**

| Feature | Free | Paid |
|---|---|---|
| [Feature 1] | ✓ | ✓ |
| [Feature 2] | Limited | ✓ |
| [Feature 3] | ✗ | ✓ |

**Conversion trigger:** [What event or friction point pushes free users toward paid? When does the paywall appear?]

**Pricing strategy observations:** [Is pricing anchored on value, competitors, or cost? Any notable pricing decisions — e.g., a free tier that's genuinely useful, a high price that signals quality, usage-based pricing that scales with value delivered?]

**Revenue quality:** [Are paying users sticky? Is revenue recurring, transactional, or event-based?]

---

## 5. Moat & Defensibility

[What makes this product hard to displace? Analyse against the following moat types:]

| Moat Type | Present? | Evidence |
|---|---|---|
| Network effects | Yes / No / Partial | [Evidence] |
| Data / personalisation | Yes / No / Partial | [Evidence] |
| Switching costs | Yes / No / Partial | [Evidence] |
| Brand | Yes / No / Partial | [Evidence] |
| Distribution | Yes / No / Partial | [Evidence] |
| Technology lead | Yes / No / Partial | [Evidence] |

**Primary moat:** [Which of the above is the strongest and most durable? Why?]

**Vulnerability:** [Where is the moat weakest? What type of entrant could disrupt this product?]

---

## 6. Key PM Takeaways

[3–5 specific, actionable insights a PM building in a similar space should extract from this teardown. Each takeaway should be a principle, not a description. Frame as: "**Principle:** [what to do/think]. **Why it works here:** [evidence from this product]. **How to apply it:** [specific application suggestion]."]

**Takeaway 1: [Principle Title]**
[Full explanation]

**Takeaway 2: [Principle Title]**
[Full explanation]

**Takeaway 3: [Principle Title]**
[Full explanation]
```

---

## Generation Instructions

- **Product Overview:** Always cite specific scale signals with a data point or note it as an estimate. "100M+ users" with no qualification is lazy. "100M+ monthly active users as of Q1 2025 (per company earnings)" is rigorous.

- **Core UX Analysis:** Be specific. Don't say "the onboarding is smooth." Say "the onboarding requires 3 steps before the first value-delivering action (generating a playlist), and each step is pre-populated with smart defaults that reduce decision fatigue."

- **Growth Loops:** Most products have 2–4 real growth loops. Don't invent loops that don't exist. If you identify fewer than 2, note that the product may be primarily paid-acquisition driven.

- **Monetisation:** Always identify the conversion trigger — the specific moment or friction point that pushes users to upgrade. This is the most revealing part of a monetisation analysis.

- **PM Takeaways:** This is the highest-value section. Be opinionated. The takeaways should be specific enough to be actionable for a PM building a similar product — not generic product management wisdom. "Don't add friction" is not a takeaway. "Trigger upgrades at task-interruption, not at session start" is a takeaway.

---

## Tone and Style

- Write like a senior PM doing a product review, not a product journalist writing a feature
- Use first-person analysis ("this mechanic works because...", "the interesting design choice here is...")
- Be willing to criticise products you admire — a good teardown identifies weaknesses, not just strengths
- Concrete > abstract. Specific > general. Principled > descriptive.
