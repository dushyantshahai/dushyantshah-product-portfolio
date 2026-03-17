# Decision Log 005: Passthrough Revenue Model

**Date:** February 2026
**Decision area:** Business model
**Status:** In progress 🔄

---

## Context

MorphEd's core operational cost is LLM inference — every assessment generation triggers an API call to Claude (MCQ generation) and, during onboarding, to Gemini (TOC extraction). As usage scales, these costs become a significant variable in the P&L. The pricing question was: **how do we structure pricing so that margins stay predictable regardless of how usage is distributed across institutes?**

Two credible models existed.

---

## Options Considered

**Option A: Flat subscription**
Institutes pay a fixed monthly or annual fee. MorphEd absorbs all LLM costs internally.
- Simple for the customer: one predictable bill, no usage tracking required
- Operationally risky: a heavy-use institute generating hundreds of assessments per month consumes disproportionate LLM costs while paying the same as a light-use institute
- Subsidisation problem: at scale, light-use institutes effectively subsidise heavy-use ones — creating a pricing model that penalises the most engaged customers and rewards disengagement
- Margin compression: as usage grows (a success signal), costs grow proportionally but revenue does not. Profitability actually gets harder the more the product succeeds.

**Option B: Base fee + token cost passthrough**
Institutes pay a platform fee for access (admin, professor, student accounts; content library; analytics) plus a per-assessment or per-token cost that reflects actual LLM consumption.
- Cost scales with consumption — high-use institutes pay more; light-use institutes pay less
- Margins stay predictable: the platform fee covers fixed costs; the passthrough component covers variable costs with a markup
- Creates natural usage transparency: professors making conscious choices about assessment volume and difficulty are implicitly managing their institute's costs
- Slightly more complex billing; requires usage tracking and reporting infrastructure

---

## Choice

**Option B — Base fee plus token cost passthrough.**

---

## Rationale

The flat subscription model has a structural flaw that becomes a crisis at scale: the more successful the product is (more assessments generated, more active professors), the more LLM costs grow, compressing margins on the very institutes driving the most value. This is the opposite of how a healthy SaaS business should work.

Passthrough pricing ties cost to consumption, which is both fair and commercially sound. An institute generating 500 assessments a month should pay more than one generating 50. The platform fee ensures baseline revenue; the passthrough component ensures that growth in usage translates to growth in revenue, not just growth in cost.

The transparency argument matters too. When institutes can see their consumption, they make better decisions — a professor choosing between 10 and 20 questions is also, implicitly, making a cost decision. That alignment between product usage and cost is healthier than hiding it behind a flat fee.

---

## Outcome / Reflection

*(Written shortly after launch — outcome is early / in progress)*

The passthrough model has been well-received in institutional conversations where it's been explained. The clearest win is the ability to offer a low-cost entry point (platform fee only for the first month) without taking on open-ended risk from heavy usage.

The open question is billing infrastructure: tracking per-assessment token consumption, generating transparent usage reports for admins, and handling billing disputes requires more operational tooling than a flat subscription. This is the next build priority on the business infrastructure side.

The model's long-term validity depends on how usage distributes across institutes as the customer base grows. The hypothesis — that heavy users pay more, covering their own costs plus a margin — will be tested properly once at least 10 institutes have been on the platform for a full billing cycle.
