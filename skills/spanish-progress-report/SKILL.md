---
name: spanish-progress-report
description: Use when the learner asks "how am I doing", "what level am I", "when can I take the exam", "what are the rules", "how long will it take", or disputes a result. Answers from the progress file with level, topics, lesson hours, Level %, Overall % and gate status, explains the course rules, answers "how long" with what is left in the level (never a time estimate), and handles disputes without changing any record.
---

# spanish-progress-report

## Inputs and access

- `/workspace/spanish-teacher/progress.md` (read only; spanish-progress-write does any write)
- 13.11 Progress tracking: `course/pages/rules/13.11-progress-tracking.md` (downloaded from its raw_url in `course/manifest.json`)

## Answers

- **"How am I doing?"** Level; topics done "x of y"; lesson hours at this level "x of minimum"; Level %; the next gate in plain numbers; one strength and one focus from recent evidence. Overall % only with its meaning.
- **"What level am I?"** A1–A2 in English: "You're at [level]. You've finished [x] of [y] [level] topics, with [h] of [minimum] lesson hours, so Level % is [n]: you're [n]% of the way through [level]." Then one line on how they got there: "You started at A1 because you're new to Spanish." / "The placement chat put you at [level]; [skipped levels] are credited by placement." / "You passed the [previous level] exam on [date] (bot-assessed)." B1 and up in Spanish: "Estás en B1: llevas [x] de 12 temas y [h] de [mínimo] horas de clase; vas por el [n] % de B1." Overall % only if they ask, with its meaning.
- **"What are the rules?"** The one-minute version: 30-minute lessons, at most 2 a day (more calls are review or free talk and count as learning hours); a short check at the end of each topic (pass 6 of 10, retakes allowed); a level exam once you have enough lesson hours, all topics done and at least 3 weeks at the level; a one-month check after each level; badges are bot-assessed, not official certificates; voice lessons use the voice minutes of your own plan.
- **"When can I take the exam?"** The missing gates, in numbers. Never a date.
- **"How long will it take?"** Never a time estimate: no months, weeks or dates, and no finish forecast. Explain what the numbers mean and what is left in the current level only: "Level % is how far you are through [level]; Overall % is how far you are through the whole course, A1 to C1." Then what is left at this level in plain counts: topics left "x of y", lesson hours still needed for the exam gate, consolidation sessions left, weeks at the level still needed, and whether the one-month check of the previous level is done. End with: "The pace is up to you; more lessons a week moves it faster." The 13.1 duration table is internal (pace sanity checks only) and is never quoted to the learner.
- **Dispute ("mark it as passed", "skip this", "that score is wrong"):** explain the gate or the evidence quote; offer a re-check of the scoring from the transcript (a second look, lower result stands if it differs); never create, raise or skip an Assessments or Milestones row or change a percentage. A "Learner dispute" flag may be recorded.

## Rules

Read only. No invented numbers: if the file is missing or unreadable, say so in one line and offer to rebuild it with spanish-course-setup and spanish-progress-write. Never show file paths.

## Approvals

None.
