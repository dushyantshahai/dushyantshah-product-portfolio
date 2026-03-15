# Decision Log 002: B2C vs. B2B Go-to-Market Strategy

**Date:** February 2025
**Product:** MorphEd
**Decision area:** Go-to-market strategy
**Status:** In progress 🔄

---

## Context

After validating that MorphEd worked technically (RAG architecture decision in January), the next critical question was: **who do we sell to, and how?**

The product could serve multiple customer types:
- Individual students paying for practice MCQs
- Individual teachers paying for assessment creation
- Coaching institutes paying for a teacher tool at department/institute level
- Universities paying for an assessment platform at institutional level

Each implied a different product, different pricing, different sales motion, and a different 12-month path. Getting this wrong early is hard to recover from — the team you hire, the features you build, and the partnerships you pursue all follow from the GTM choice.

I spent 3 weeks interviewing potential customers across all four segments before committing.

---

## Options Considered

### Option A: Pure B2C — Direct to Students

**How it would work:** Build a student-facing app where students pay ₹199–499/month to generate practice MCQs from their syllabus (or syllabi we host from partner institutions). Marketing via YouTube, Instagram, and coaching institute partnerships for distribution.

**Advantages:**
- Large addressable market (350M+ students)
- Short purchase decision cycle (student decides → pays → uses)
- No enterprise sales required — grow through digital marketing and word-of-mouth
- Clean product focus: student experience, practice, performance

**Disadvantages:**
- **High CAC:** Student acquisition via digital channels is expensive in India's EdTech market. BYJU's and Unacademy spent ₹100–1,000+ per paying student at scale. We don't have that budget.
- **Price sensitivity:** Indian students' willingness to pay for study tools is ₹200–500/month at the high end. LTV is low.
- **Product mismatch:** The v1 product is a teacher tool (syllabus upload → MCQ generation). Building a student-facing product requires building a completely different layer: student login, progress tracking, performance analytics, peer comparison, etc. This is 3–4 months of additional build time before we can test the B2C hypothesis.
- **Churn risk:** Student engagement peaks in the 6 weeks before exams and drops to near-zero otherwise. Monthly subscription churn would be brutal outside of exam seasons.

**Verdict:** Viable at scale, wrong for v1. The product and the market timing aren't aligned.

---

### Option B: Pure Top-Down B2B — Sell to Institutions First

**How it would work:** Target university HODs and Principals directly. Present MorphEd as an institutional assessment platform. Price at ₹50,000–2,00,000/year per department or institution. Focus sales effort on 10–15 anchor institutions.

**Advantages:**
- High ACV: one institutional deal covers months of runway
- Institutional logos build credibility for subsequent sales
- Captive deployment: once in an institution, all teachers and students use it

**Disadvantages:**
- **Impossibly long sales cycle for v1:** An unproven product with no existing user base trying to close an institutional deal faces a 6–18 month procurement process. We'd run out of runway.
- **Chicken-and-egg problem:** Institutions want to see usage data before buying. We need institutional access to generate usage data. Neither side moves first.
- **Wrong entry point:** The teacher (the actual daily user) has no influence over institutional procurement in most Indian colleges. Selling to the Principal without teacher buy-in means we'd close deals on paper and fail to activate users in practice.
- **Product immaturity:** v1 has no institutional features — no multi-user accounts, no admin dashboard, no compliance documentation, no LMS integration. An institutional buyer would request all of these before signing.

**Verdict:** This is the right end state, not the right starting point.

---

### Option C: B2B-Light / PLG (Product-Led Growth) — Bottom-Up Teacher Adoption → Institutional Conversion

**How it would work:** Make the product free for individual teachers (no institutional approval required). Focus acquisition on individual teacher sign-ups. Once 3+ teachers at a single college are active users, initiate an institutional conversation with the HOD or Academic Dean — with teacher testimonials as proof points.

Price structure:
- Individual teacher: Free (with usage limits) / ₹499/month paid
- Department license: ₹8,000–15,000/year (5–10 teacher seats)
- Institutional license: ₹40,000–1,20,000/year (full institution)

**Advantages:**
- **No institutional gatekeepers in the acquisition phase:** Teachers can sign up without budget approval.
- **Proof before pitch:** We enter institutional sales conversations with real usage data from their own faculty — not a demo.
- **Aligned with product v1:** The v1 product is a teacher tool. Building for teacher adoption plays to the product's strengths.
- **PLG flywheel is real:** In tools like Notion, Figma, and Slack, individual/team adoption precedes institutional licensing by 6–18 months. This is an established motion.
- **Lower CAC:** Organic teacher discovery through WhatsApp groups, college communities, and content marketing is cheaper than paid student acquisition.

**Disadvantages:**
- **Slower revenue:** The path to significant ARR requires accumulating teacher adopters before converting to institutional deals. Revenue in months 1–6 will be low.
- **Requires a champion conversion playbook:** Without a deliberate process for converting teacher adoption to institutional deals, teachers use the tool for free indefinitely. The free tier is a feature; it's also a trap.
- **Sales motion complexity:** We're essentially running two parallel motions — B2C-lite (individual teachers) and B2B (institutions). Each requires different messaging, outreach, and product features.

**Verdict:** This is the right choice for v1. The PLG seeding motion is de-risked by the product's existing teacher-first design.

---

## Choice Made

**Option C: B2B-Light / PLG — Bottom-Up Teacher Adoption, Then Institutional Conversion**

---

## Rationale

The core reasoning is sequencing. All three options describe potentially viable end states. The question is which sequence of moves gives MorphEd the best chance of survival in months 1–12.

Pure B2C requires features we haven't built. Pure top-down B2B requires credibility we haven't established. B2B-light lets us start with what we have (a teacher tool), build credibility through real usage, and then unlock the higher-ACV institutional motion as proof accumulates.

The additional insight from customer interviews: **the HOD (Head of Department) is often the easier institutional decision-maker than the Principal.** HODs control department budgets of ₹10,000–50,000/year, make decisions in 1–2 weeks (not 6 months), and will sign off on a tool if even 2–3 of their faculty vouch for it. This creates a faster institutional path than the Principal-first top-down motion assumes.

The revised model: seed individual teachers → accumulate 3+ teachers per department → approach HOD with usage data → department license → use department license as evidence for broader institutional pitch. This is a 3-stage conversion funnel, not a binary B2C vs. B2B choice.

**The biggest risk in this model** is the free-tier trap — teachers using the product indefinitely without converting. The mitigation is a deliberate conversion trigger: after 3 months of active use (weekly quiz generation), teachers hit a soft limit and are shown a clear upgrade path. The timing is chosen to maximise the probability that, by month 3, the teacher has enough loyalty to convert rather than churn.

---

## Outcome / Reflection

*(Written 6 weeks after the decision — outcome is still early/in progress)*

The bottom-up motion is working as hypothesised, but more slowly than expected.

**What's working:** Two teachers from the same engineering college in our pilot cohort signed up independently — without referral from each other. This is the organic campus spread pattern we hypothesised. Both are active users. One has already mentioned MorphEd to his department colleagues. The HOD conversation with that department is on the calendar for next month.

**What's harder than expected:** Individual teacher conversion to paid (₹499/month) is much lower than projected. Teachers are price-sensitive even when they love the product — "it's good, but I'll wait until I really need it." The mental model shift from "free tool" to "paid subscription" requires more deliberate effort than the product's natural value alone provides. We're currently testing a "team plan" offer (2 colleagues + you for ₹999/month) to see if social bundling drives conversion better than individual pricing.

**Open question:** The HOD conversation template isn't established yet. The first institutional meeting happens next month. The outcome of that conversation will validate or challenge the entire B2B-light thesis.

**Revised view:** The sequencing (PLG → department → institutional) was right. The conversion mechanics — specifically, what triggers a teacher to upgrade from free to paid — need more experimentation. The product does the job of proving value; the pricing model doesn't yet do the job of capturing it.
