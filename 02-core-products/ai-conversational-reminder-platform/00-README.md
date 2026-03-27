# RemindMe — AI Conversational Reminder Platform

**Overview:**
RemindMe is a consumer-facing AI reminder app that replaces traditional form-based reminder creation with a natural, conversational setup flow. Instead of filling in fields for title, date, time, and notes, users simply type what they need to remember — and an AI assistant asks the right follow-up questions to fill in the gaps, automatically enriching each reminder with structured context, theme tags, and urgency classifications.

The product was built end-to-end: from product discovery and architecture to a fully deployed, production application on Vercel with Google OAuth, PostgreSQL, Firebase push notifications, and a dual-LLM pipeline (Gemini 2.5 Flash as primary, Claude 3 Haiku as fallback).

**Documentation Structure:**
The project includes seven detailed documents covering distinct aspects:

- **problem.md** addresses the cognitive friction in traditional reminder apps and the market opportunity for conversational AI reminders
- **users.md** profiles three distinct user personas and their core reminder needs
- **solution.md** outlines the conversational creation flow, reminder lifecycle, notification system, and AI enrichment features
- **ai-architecture.md** describes the dual-LLM pipeline, ConversationManager state machine, context extraction engine, and time-parsing strategy
- **product-decisions.md** chronicles six significant architectural and product choices and their underlying rationale
- **metrics.md** establishes the North Star Metric, OKR framework, and guardrail metrics for the product
- **roadmap.md** tracks V1 completion and maps the V2 feature vision

**Author & Attribution:**
Created by Dushyant Shah. The work remains unpublished for redistribution, with copyright held through 2026.

[Return to main portfolio](../../README.md)
