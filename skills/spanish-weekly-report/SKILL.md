---
name: spanish-weekly-report
description: Use when the weekly report routine runs or when the learner asks for their weekly summary. Writes a Weekly log row with the progress-file write procedure and shows the week in chat: level, topics done, Level %, Overall % with its meaning, hours, lessons planned and met, weeks on plan, assessments, pending results, gate status and next week's focus.
---

# spanish-weekly-report

## Inputs and access

- `/workspace/spanish-teacher/progress.md`; spanish-progress-write (write procedure and formulas)
- Optional: a Page for the learner, only if they asked for one (a view, never the record)

## Sequence

1. Run spanish-progress-write in reconcile mode first so the week is complete.
2. Compute for the week (Monday–Sunday in the learner's timezone): planned lessons (Profile), lessons met, lesson hours, learning hours, learning hours to date, weeks on plan (consecutive weeks with lessons met ≥ 80% of planned), level, topics done "x of y", writing tasks "x of n" (typed track), Level %, Overall %, assessments this week, pending results, gate status, focus next week (top error).
3. Write the Weekly log row (one per week; never twice for the same week).
4. Show in chat, short, headings in English at A1–A2 and Spanish from B1:
   - Level and Level %; Overall % with its meaning: "Overall % is how far you are through the whole course, A1 to C1. It moves slowly because the course is 328 hours of lessons."
   - Hours this week and to date ("all of it helps your fluency")
   - Lessons planned and met, weeks on plan
   - Results this week; anything pending
   - Next gate in plain numbers
   - Focus for next week
   - Milestone messages not yet given (once each, as Encouragement rows)
5. No data this week: one line ("No lessons this week. Want a 15-minute review tomorrow?"). Never invent activity.

## Rules

Percentages are never rounded up or estimated. No target dates. Never send the report anywhere outside this chat without a yes.

## Approvals

None for the chat message. Making a Page needs the learner's yes once.
