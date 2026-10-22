---
name: spanish-review
description: Use when the learner asks to review or practise words, when items are due in the Review queue, for a third call in one day, for a short 15-minute session on a busy day, or when the session brief lists overdue items. Runs out-loud spaced review of due items and updates their steps.
---

# spanish-review

Rules: 13.10 Spaced review schedule, `course/pages/rules/13.10-spaced-review-schedule.md` (downloaded from its raw_url in `course/manifest.json`).

## Inputs and access

- Review queue in `/workspace/spanish-teacher/progress.md` (or the due list in the latest session brief if the file can't be read in a call)
- The learner's study days (Profile)

## Sequence

1. Pick due items: oldest overdue and problem items (3+ misses) first, then the rest, up to the study day's share (weekly budget ÷ study days; budget 168 a week at A1–A2, 210 at B1–C1; about 15–20 seconds an item).
2. Prompt out loud, no blanks: A1–A2 English meaning → Spanish; B1–C1 a Spanish definition, a gap in a spoken frame read as a full question, or a transformation. One answer per item.
3. Grade:
   - Correct and quick → up one step
   - Correct after a hint → same step
   - Missed at step 1–3 → back to step 1, asked again later in the same call
   - Missed at step 4–5 → down two steps
   - Missed 3 times in total → problem item: 2-minute mini-lesson in the next lesson
4. Next due date by step: 1 → next study day; 2 → second study day after; 3 → same weekday next week; 4 → 21 days; 5 → 60 days (calendar, moved to the next study day); after step 5 → Done, moved to the archive (still sampled in exam memory checks and one-month checks).
5. Overdue rule: if overdue items exceed one study day's share, tell the learner how long catching up takes (about 17 seconds an item) and new active items in lessons drop to 2 per session until overdue falls below half a share.
6. Short session (15 minutes): 2-minute opening, 10 minutes of due review, 3-minute mini-practice of today's topic. It is a Review row, not a lesson.
7. spanish-progress-write: Sessions row type Review, queue steps and due dates, archive moves.

## Return

"Reviewed 34 items: 29 right. Next review: Thursday, about 12 minutes." No percentages beyond what spanish-progress-write computes.

## Approvals

None.
