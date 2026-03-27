# RemindMe Solution Design

RemindMe is a web-based AI reminder app that replaces form-based reminder creation with a conversational setup flow — designed to get a richly structured reminder saved in under 60 seconds, with no fields to fill.

## Core User Flow

**Reminder Creation:** The user navigates to the Create Reminder page and types a plain-language statement — anything from "remind me to call mom tonight" to "remind me to pay home loan EMI, it's due on the 20th every month." The app immediately creates a draft reminder and launches the AI conversation. The assistant asks targeted follow-up questions (up to 5 turns maximum) to fill in missing context: time, date, recurrence pattern, relevant participants, or location. Once the conversation completes — either via explicit user confirmation, a keyword like "just set it", or the hard 5-turn limit — the reminder is saved as active with all extracted context persisted to the database.

**Reminder Dashboard:** Users see all their active, snoozed, and completed reminders in a unified list, with theme tags (Health, Finance, Work, Family, Shopping, Entertainment, Social, Transport, Travel, Personal) and urgency classifications (Urgent & Important / Urgent but Not Important / Important but Not Urgent / Not Important & Not Urgent) surfaced inline. This Eisenhower-style urgency matrix allows at-a-glance prioritization without any manual tagging by the user.

**Notification Delivery:** A Vercel cron job runs on a defined schedule and queries for all active reminders whose `reminder_time` has arrived. For each due reminder, it fires both an in-app notification (displayed in the notification bell) and a push notification via Firebase Cloud Messaging (FCM) to all registered devices for that user. Users can act on notifications directly — marking reminders as complete, snoozed, or dismissed.

## Key Design Principles

The product is built around three explicit values: **speed over completeness** (a 30-second conversational setup beats a 3-minute form), **assistant intelligence over user burden** (the AI should infer what it can — proposing "7 PM tonight?" rather than asking "what time?"), and **structured output from unstructured input** (every conversation produces a typed data record, not a free-text blob).

The assistant is designed to sound like a helpful friend, not a corporate chatbot. It uses lowercase soft confirmations ("got it", "sure", "okay"), proposes specific times rather than asking open-ended questions, and ends conversations with domain-appropriate warm closers ("enjoy your pizza!", "stay hydrated!", "hope you enjoy the movie!"). The conversation style rules enforce a strict zero-repetition policy and forward-momentum requirement — every response must either add new information or close the conversation.

## Deliberate Scope Limitations

V1 excludes recurring reminder automation (recurrence is captured in context but requires manual re-creation), voice input, calendar sync as a write-back (Google Calendar is read-only for context hints), shared/collaborative reminders, and native mobile apps. Each exclusion has a clear V2 unlock condition tied to validating core engagement first.
