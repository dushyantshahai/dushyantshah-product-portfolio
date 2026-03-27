# RemindMe AI Architecture

## Overview

RemindMe's AI layer is built around three coordinated pipelines: a **Conversational Response Engine** that drives the reminder creation dialogue, a **Context Extraction Engine** that converts completed conversations into structured data records, and a **Time Parsing Engine** that resolves natural language time expressions to precise ISO 8601 timestamps. All three pipelines share a common dual-LLM infrastructure with automatic fallback.

## Dual-LLM Infrastructure

The system uses **Gemini 2.5 Flash** as the primary model and **Claude 3 Haiku** as the fallback. Every LLM call routes through a `runWithFallback` wrapper: Gemini is called first; if its response fails a JSON validity check or throws an error, the identical prompt is re-issued to Claude. If both fail, the error surfaces to the caller with a structured fallback response. This design ensures zero single-point-of-failure on the AI layer without requiring the complexity of a load balancer or queue.

The JSON validity check (`detectLLMFailure`) looks for a valid JSON start/end boundary (`{` / `}`) and attempts to parse the substring — any response that can't be parsed as JSON is treated as a failure and triggers the fallback.

## Conversational Response Engine

The response engine is implemented in `llmService.generateAssistantResponse()` and uses a **think-step prompting** approach: the system prompt instructs the LLM to reason silently through seven questions before generating any output — what action the user is asking for, whether an event date differs from the reminder date, what information is already known from the conversation history, what the single most important missing piece is, and whether a specific time can be inferred rather than asked. The reasoning step is never included in the output; only the final conversational message is returned.

The prompt encodes a **domain reasoning layer** with specialized logic for nine reminder categories: movie/event tickets (event date ≠ reminder date), groceries and shopping (timing + offer list), food orders (venue or delivery detail), recurring health reminders (compute distribution pattern), work tasks (infer PM for ambiguous times), bills and EMIs (due date ≠ reminder date, proactively suggest reminding earlier), documents (ask for recipient), calls and social plans (suggest specific time for vague inputs), and daily habits (confirm recurrence + suggest reasonable time).

A **computation engine** within the prompt handles time distribution patterns: when a user says "6 times between 7am and midnight," the LLM is instructed to calculate the interval (17 hours ÷ 5 gaps = 3h 24m), compute all six fire times, and present the full computed schedule for confirmation rather than asking the user to figure it out.

## Context Extraction Engine

After conversation completion, `llmService.extractContext()` passes the full conversation history to the LLM and requests a structured JSON object with 11 fields: `reminder_time`, `reminder_date`, `is_recurring`, `recurrence_pattern`, `reminder_count`, `frequency_description`, `location`, `participants`, `notes`, `theme_tag`, and `urgency_tag`. The extraction prompt instructs the model to use the *confirmed* time from the conversation (not raw user input), compute absolute dates from relative expressions like "tomorrow" or "day after tomorrow," and write a value-adding `notes` field rather than copying the reminder title.

The raw LLM output is passed through `applyContextTagHeuristics()` — a post-processing layer that normalizes both theme and urgency tags using regex-based keyword matching. This guards against LLM theme/urgency drift: even if the model returns an unrecognized tag, the heuristic layer enforces membership in the canonical 10-theme taxonomy and 4-level Eisenhower urgency matrix. The urgency normalizer uses temporal proximity (is the reminder within 24 hours?), domain criticality (health/finance/work signals), and consequence signals (deadline, penalty, emergency keywords) to assign urgency correctly.

## Time Parsing Engine

`llmService.parseReminderTime()` resolves a natural language time expression to a `Date` object using a three-stage pipeline: **pattern matching → LLM parsing → default fallback**. The pattern matcher handles the most common expressions (specific times like "3 PM", relative durations like "in 2 hours", named times of day like "morning"/"evening"/"midnight") with logic to decide between today and tomorrow based on whether the target time is still in the future. Only if pattern matching fails does the system escalate to an LLM call with a structured prompt and context inference rules. The default fallback — 9 AM tomorrow — fires only if both prior stages fail, ensuring the system never blocks reminder creation on a parsing error.

## Acknowledged Limitations

The V1 conversation engine does not natively enforce recurring reminder *delivery* — recurrence is captured in context but the cron job delivers each reminder as a one-time fire. The context extractor cannot reliably decompose multi-part reminders ("remind me to do X and Y at different times"). The time parser may misinterpret highly ambiguous inputs without conversational context. All three represent planned V2 improvements.
