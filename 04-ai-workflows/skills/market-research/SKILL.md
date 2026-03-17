# SKILL: Market Research Synthesiser

**Name:** market-research
**Version:** 1.0
**Author:** Dushyant Shah
**Description:** Synthesises a competitive landscape and user insights for a given product domain. Outputs a structured market map, competitor comparison, and whitespace opportunity analysis — designed for PMs who need rapid market context before a strategy or positioning conversation.

---

## When to Invoke This Skill

Invoke this skill when the user:
- Asks for a competitive analysis of a market or product category
- Wants to understand where their product sits in a landscape
- Is preparing for a product strategy, positioning, or investor conversation
- Wants to identify market gaps or whitespace opportunities
- Says anything like "research the [X] market" or "who are the competitors for [Y]"

Do not invoke this skill for single-product teardowns — use the `product-teardown` skill instead.

---

## Behaviour When Invoked

### Step 1: Clarify Scope (if not already provided)

Ask in a single message:

1. **Domain / Market:** What is the product category or market to research? (e.g., "AI-powered EdTech for Indian colleges", "B2B SaaS project management tools")
2. **Your product / vantage point:** Do you have a product in this space? If so, briefly describe it. This helps frame the competitive analysis relative to your position.
3. **Geography and customer segment:** Any geographic or segment constraints? (e.g., India only, SMB-focused, K-12 vs. higher education)
4. **Depth needed:** Quick landscape overview or deep-dive on specific competitors?

If the user has provided enough context, skip to Step 2.

### Step 2: Generate the Market Research Output

Produce the research document in the following structure:

---

## OUTPUT TEMPLATE

```markdown
# Market Research: [Domain / Market Name]

**Prepared for:** [User's product or company, if provided]
**Date:** [Today's date]
**Scope:** [Geography, segment, product category]

---

## 1. Market Overview

**What is this market?**
[2–3 sentences defining the space: what problem is being solved, who the customers are, and what category of solutions exists.]

**Market size and growth:**
[TAM/SAM/SOM estimates if reasonably estimable. Note confidence level and sources. If data is unavailable, provide a directional estimate with reasoning.]

**Key market dynamics:**
[2–4 bullet points on structural forces shaping this market: regulatory shifts, technology inflections, buyer behaviour changes, supply-side dynamics]

---

## 2. Market Map

[Categorise competitors into segments. Use a 2x2 or category matrix format in text/markdown. Example axes: specialised vs. general, B2C vs. B2B, established vs. emerging.]

```
          SPECIALISED
               │
    [Player A] │ [Player B]
               │
B2C ───────────┼─────────────── B2B
               │
    [Player C] │ [Player D]
               │
            GENERAL
```

[Brief explanation of each quadrant and which segment is most contested]

---

## 3. Competitor Comparison

| Competitor | Core Value Prop | Target Customer | Pricing | Strengths | Weaknesses |
|---|---|---|---|---|---|
| [Competitor 1] | | | | | |
| [Competitor 2] | | | | | |
| [Competitor 3] | | | | | |
| [Your Product] | | | | | |

[Include the user's product in the table for direct comparison. Mark it clearly.]

---

## 4. Deep Dives: Top 3 Competitors

For each of the 3 most relevant competitors, provide:

### [Competitor Name]

**What they do:** [1–2 sentences]
**Business model:** [How they make money]
**Key differentiator:** [What they do uniquely well]
**Traction signals:** [User base, funding, revenue estimates, notable customers if public]
**Vulnerability:** [Where they're weak or where they're not serving the market well]

---

## 5. User Insights (Synthesised)

[If user research data is provided, synthesise it here. If not, use public signals (app store reviews, Reddit, Twitter/X, ProductHunt comments) to surface common themes.]

**What users love about current solutions:**
- [Theme 1]
- [Theme 2]

**What users consistently complain about:**
- [Theme 1]
- [Theme 2]

**Unmet needs not addressed by current solutions:**
- [Insight 1]
- [Insight 2]

---

## 6. Whitespace Opportunities

[3–5 specific gaps in the market — areas where user needs are unmet, competitor positioning is weak, or a technology/regulatory shift has created a new opening.]

| # | Opportunity | Evidence | Who's Best Positioned to Capture It |
|---|---|---|---|
| 1 | [Opportunity] | [Why this gap exists] | [Who/what type of player] |
| 2 | | | |

---

## 7. Strategic Implications for [User's Product]

[Tailored section if the user has described their product. Answer: given this landscape, what does this mean for positioning, feature prioritisation, and go-to-market?]

**Positioning recommendation:** [Where in the market map should the product sit, and why?]
**Competitive moats to build:** [What should the product invest in to be hard to displace?]
**Risks to watch:** [Which competitor moves or market shifts could threaten the product?]
```

---

## Generation Instructions

- **Market Map:** Always attempt a 2x2 or category-based visual. If the space doesn't lend itself to a 2x2, use a category list (Established players / New entrants / Adjacent players).

- **Competitor Comparison Table:** Include the user's product in the table — even if the user didn't explicitly ask for it. Seeing their product beside competitors is the most useful output of this skill.

- **Whitespace Opportunities:** Don't list generic observations like "mobile-first" or "AI integration." Each opportunity should be specific enough that it could become a product thesis: "No existing tool offers [X] for [specific segment], despite [evidence of demand]."

- **Strategic Implications:** This section requires judgment, not just information synthesis. Make a recommendation. Be opinionated. The user can push back — a blank "it depends" adds no value.

- **Data limitations:** When market data is uncertain, state your confidence level: "Estimated $X–Y TAM based on [reasoning]" rather than asserting unverified figures.

---

## Tone and Style

- Analytical, opinionated, and direct
- Write for a PM or founder, not a consultant — skip the executive summary boilerplate
- Use tables and structured lists for comparisons; use prose for analysis and implications
- Synthesise, don't just list — the value is in the pattern recognition, not the raw data
