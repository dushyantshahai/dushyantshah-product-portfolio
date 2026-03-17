# SKILL: Market Research Synthesiser

**Name:** market-research
**Version:** 1.1
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

1. **Domain / Market:** What is the product category or market to research? (e.g., "AI-powered assessment tools for Indian colleges", "B2B SaaS project management tools")
2. **Your product / vantage point:** Do you have a product in this space? If so, briefly describe it. This helps frame the competitive analysis relative to your position.
3. **Geography and customer segment:** Any geographic or segment constraints? (e.g., India only, coaching institutes vs. degree colleges, K-12 vs. higher education)
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
[Directional estimate only — see data confidence rules in Generation Instructions. Flag the source and confidence level explicitly. Do not assert specific figures without citing a named source.]

**Key market dynamics:**
[2–4 bullet points on structural forces shaping this market: regulatory shifts, technology inflections, buyer behaviour changes, supply-side dynamics]

---

## 2. Market Map

[Position competitors using the most revealing axes for this space. For Indian B2B EdTech assessment tools, use the axes below as the default. Adjust for other markets.]

```
         AI-NATIVE
              │
   [Player A] │ [Player B]
              │
B2C ──────────┼─────────────── B2B-INSTITUTE
              │
   [Player C] │ [Player D]
              │
         TRADITIONAL
```

[Brief explanation of each quadrant, which is most contested, and where your product sits. Mark your product clearly with ★.]

---

## 3. Competitor Comparison

| Competitor | Core Value Prop | Target Customer | Pricing Model | Strengths | Weaknesses |
|---|---|---|---|---|---|
| [Competitor 1] | | | | | |
| [Competitor 2] | | | | | |
| [Competitor 3] | | | | | |
| ★ [Your Product] | | | | | |

[Always include the user's product in the table. Mark it clearly with ★.]

---

## 4. Deep Dives: Top 3 Competitors

For each of the 3 most directly threatening competitors, provide:

### [Competitor Name]

**What they do:** [1–2 sentences]
**Business model:** [How they make money]
**Key differentiator:** [What they do uniquely well]
**Traction signals:** [Funding rounds, named partnerships, or press mentions — see traction rules in Generation Instructions]
**Vulnerability:** [Where they're weak or not serving the market well — this is where your product wins]

---

## 5. User Insights

> ⚠️ **Data policy for this section:** Only synthesise from data the user has provided (interviews, surveys, support tickets, NPS comments). Do NOT generate user sentiments from general LLM knowledge or claim to source from Reddit/Twitter/App Store without live access to those platforms. If no user data has been provided, state that clearly and leave placeholders for the user to fill in.

**What users value about current solutions:**
- [Theme 1 — from provided data, or leave as placeholder]
- [Theme 2]

**What users consistently complain about:**
- [Theme 1]
- [Theme 2]

**Unmet needs not addressed by current solutions:**
- [Insight 1]
- [Insight 2]

---

## 6. Whitespace Opportunities

[3–5 specific gaps — areas where user needs are unmet, competitor positioning is weak, or a technology/regulatory shift has created a new opening. Each opportunity must be specific enough to become a product thesis.]

| # | Opportunity | Evidence | Who's Best Positioned |
|---|---|---|---|
| 1 | [Opportunity] | [Why this gap exists — regulatory, behavioural, or technical] | [What type of player] |
| 2 | | | |

---

## 7. Strategic Implications for [User's Product]

[Answer three questions with a clear point of view — no "it depends":]

**Positioning recommendation:** [Where in the market map should the product sit, and why? What's the one-line positioning statement that follows from this analysis?]
**Competitive moats to build:** [What should the product invest in over the next 12–18 months to be hard to displace?]
**Risks to watch:** [Which competitor moves or market shifts — funding rounds, product expansions, regulatory changes — could threaten the product in the next 12 months?]
```

---

## Generation Instructions

### MorphEd Market Context (Pre-Loaded)

When the research is for MorphEd or the Indian B2B EdTech assessment market, use this pre-loaded context rather than generating from scratch:

**Market:** B2B AI-powered MCQ assessment tools for Indian educational institutes (coaching centres, junior colleges, degree colleges — Std 11 and above, Commerce / Arts / Science streams).

**Known competitors:**

| Player | Category | What they do | Relevance to MorphEd |
|---|---|---|---|
| Embibe | AI-native, B2C-first | Personalised learning and adaptive assessment — primarily student-facing, enterprise deals with state governments | Largest AI EdTech in India; B2B route is via large state contracts, not SMB institutes |
| Extramarks | Traditional → AI | LMS + content + assessment for K-12 and higher ed; large sales force | Feature-heavy, expensive, complex to deploy; not AI-native in generation |
| Testbook | B2C, competitive exam | Competitive exam prep (SSC, banking, UPSC) — high-volume MCQ practice for individual learners | Different segment (competitive exam vs. curriculum-based); student-facing only |
| Google Forms | Manual alternative | Free form builder used by professors for ad-hoc assessments | The real incumbent — the baseline MorphEd must beat on speed and quality |
| DigiExam / similar | Traditional assessment | Proctored online exam delivery; no AI generation | Exam delivery focus, not generation; different job to be done |

**Known market dynamics:**
- NEP 2020 mandates continuous competency-based assessments — creates compliance urgency in private colleges
- NAAC reaccreditation pressure requires documented assessment quality and learning outcome records
- Coaching institute segment (JEE/NEET/CA) already runs high-frequency assessments — most natural early adopters
- AI-adoption window: institutes building curriculum-specific RAG knowledge bases now compound their advantage over late movers

**ICP (already validated):**
- Tier 1: Coaching institutes — owner/director is decision-maker, faster cycle, clearest ROI on professor time
- Tier 2: Private engineering/commerce colleges — bottom-up via professor champion, NAAC as urgency hook
- Defer: Government colleges, state boards — complex procurement, long cycles

**Default Market Map axes for MorphEd:**
- X-axis: Traditional ←→ AI-Native
- Y-axis: B2C-Student ←→ B2B-Institute
- MorphEd sits: AI-Native + B2B-Institute quadrant (top-right). This quadrant is currently the least contested — use this as the strategic framing.

### Market Size and TAM/SAM/SOM — Data Confidence Rules

Never assert specific market size figures without a named source. Apply this standard:

- If the user provides a source (IBEF, RedSeer, Redseer, Inc42 India EdTech report), use it and cite it.
- If no source is provided, generate a **directional estimate** with explicit reasoning: "Rough directional estimate: India has ~50,000 private coaching institutes and ~4,000 private colleges. At an average contract value of ₹25,000–1,00,000/year, the addressable SMB segment is ₹X–Y Cr annually. This is an internal sizing estimate, not a cited figure — verify before using in investor conversations."
- Never use round figures presented as facts (e.g., "India EdTech market is $10.4B") without sourcing them. Flag all AI-estimated market figures with: ⚠️ *Directional estimate — verify before citing externally.*

### User Insights — No Fabrication Rule

The User Insights section must only draw from data the user has explicitly provided. Do not:
- Claim to have read App Store reviews, Reddit, or Twitter/X unless you have live access to those platforms in the current session
- Generate "common themes" from general LLM knowledge about what EdTech users typically say
- Present synthesised sentiment as if it came from research

If no user research data has been provided, write: *"No user data provided for this section. Placeholder left for the user to fill in from interviews, NPS comments, or support data."* Then optionally suggest 2–3 specific questions the user could ask in professor or admin interviews to surface the insights this section needs.

### Competitor Traction Signals — Indian Private Company Rule

Revenue figures and user base numbers for Indian private EdTech companies (Embibe, Extramarks, Testbook, etc.) are rarely public and are frequently inaccurate in AI training data. Apply this standard:

- **Use:** Named funding rounds (e.g., "raised $188M Series E — Softbank-backed"), known government or state-level partnerships, publicly announced customer counts or geographies
- **Avoid:** Revenue estimates, DAU/MAU figures, market share percentages — unless sourced from a named recent report
- If traction data is unavailable, write: *"No public traction data available — directional signals only."* Do not fill the gap with plausible-sounding invented numbers.

### Freshness Warning

The Indian EdTech competitive landscape shifted materially between 2022 and 2025 — funding contraction, consolidation, and the emergence of AI-native tools have changed who the relevant players are. Before finalising the competitor section, prompt the user: *"This analysis is based on training data with a knowledge cutoff. Please confirm whether any major funding rounds, acquisitions, shutdowns, or new entrants have occurred in this space in the last 12 months before using this output in a strategy conversation."*

### Whitespace Opportunities

Don't list generic observations like "mobile-first" or "AI integration." Each opportunity should be specific enough that it could become a product thesis: "No existing tool offers [X] for [specific segment], despite [evidence of demand]."

### Strategic Implications

This section requires judgment, not just information synthesis. Make a recommendation. Be opinionated. The user can push back — a blank "it depends" adds no value.

---

## Tone and Style

- Analytical, opinionated, and direct
- Write for a PM or founder, not a consultant — skip the executive summary boilerplate
- Use tables and structured lists for comparisons; use prose for analysis and implications
- Synthesise, don't just list — the value is in the pattern recognition, not the raw data
- Flag data confidence honestly — a well-labelled uncertain insight is more useful than a confident hallucination
