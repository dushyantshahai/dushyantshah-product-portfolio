# User Personas — RemindMe

RemindMe serves three distinct user archetypes, each with different reminder patterns and expectations from an AI assistant:

## The Busy Professional (Arjun Mehta)

Arjun is a 31-year-old product manager at a mid-sized tech company in Bengaluru. His day is packed with context switching — meetings, deliverables, client calls, and end-of-day reports. He sets reminders on the go, often while doing something else, and needs the process to be as fast as dictating a note. His core frustration with existing apps: "I have to stop what I'm doing to fill in fields. By the time I've opened the app and tapped through date-time pickers, I've lost focus on what I was actually doing."

RemindMe addresses Arjun's need for speed through conversation: he types "remind me to send the financial analysis to Rahul at 5pm today" and the assistant confirms in one exchange — no fields, no taps, no date pickers. The AI correctly infers PM for work-hour times, understands deadlines vs. reminder times, and stores structured context so his reminder dashboard reads like an organized task list rather than a pile of sticky notes.

## The Health and Habits User (Priya Sharma)

Priya is a 27-year-old who tracks her health closely — water intake, vitamins, evening walks, and medication. She's tried habit apps but finds them either too rigid (fixed intervals) or too manual (she has to set up each reminder individually). Her core need: "I want to tell it to remind me to drink water 6 times between 7am and midnight, and have it figure out the schedule for me."

RemindMe's computation engine handles exactly this: it calculates the even distribution (5 gaps across 17 hours = every 3h 24m), presents the computed schedule for confirmation, and stores all 6 as a single recurring reminder cluster. The urgency tagger automatically marks health reminders as "Important" by default, ensuring they surface with appropriate priority on her dashboard.

## The Finance-Aware User (Vikram Nair)

Vikram is a 38-year-old small business owner who manages multiple financial obligations — home loan EMI, credit card dues, vendor payments, and quarterly tax filings. His pain point is the gap between when a bill is *due* and when he needs to *act*: "I always set a reminder for the due date itself, and then I'm scrambling to transfer funds last minute."

RemindMe proactively bridges this gap. When Vikram says "remind me to pay EMI on the 20th," the assistant doesn't just record the date — it recognizes the pattern (payment due date), suggests reminding him 2 days earlier on the 18th, asks what the EMI is for to enrich the notes, and stores it as a monthly recurring reminder. The urgency tagger classifies it as "Important but Not Urgent" when set in advance, automatically shifting to "Urgent & Important" as the date approaches.

Each persona drives a distinct product requirement: speed and intelligence for professionals, computation and recurrence for health users, and proactive gap-filling for finance users — together shaping RemindMe's core conversational design.
