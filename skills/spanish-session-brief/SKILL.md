---
name: spanish-session-brief
description: Use before each planned study day (run by the session reminder routine), at the start of any day-two-onward conversation, or when the learner asks "what's next" or "what do we do today". Saves any unsaved call first, then posts one short brief: topic and session, due reviews, pending retakes, gate status, level and Level %.
---

# spanish-session-brief

The brief is posted in chat so a lesson never depends on reading the file during a live call.

## Inputs and access

- `/workspace/spanish-teacher/progress.md` (Profile, Sessions, Assessments, Review queue, Milestones)
- `course/status.md` (is today's topic page OK?) and `course/last-version-check.txt` (when the pack version was last checked)

## Sequence

1. If a call since the last Sessions row has no row, or a check result is pending, run spanish-progress-write first.
   - If `course/last-version-check.txt` is missing or more than 7 days old, run the spanish-course-setup version check (a few seconds); if an update is needed, run spanish-course-setup now, or after the call if a lesson has started.
2. If today is not a study day in the Profile, do nothing (routine runs) or answer briefly (when asked).
3. Build the brief:
   - Today: course · topic title · session n of N (or "topic check", "consolidation", "fast-track offer")
   - Review: number of due items and minutes (about 17 seconds an item), up to 8 in the lesson
   - Pending: retakes and their earliest date; any result still pending
   - Gates: the next gate in plain numbers ("A1 exam: 18 of 24 lesson hours, 4 of 6 topics")
   - Level and Level %
   - One line: "Start a voice call when you're ready; I'll begin with «¿Seguimos con [tema], sesión [n]?»"
4. If today's topic page is not OK in the cache, run spanish-course-setup for it; if still not OK, say so and offer review instead.
5. Day-two opening (when the learner writes first): exactly one line, e.g. "Today: A1.2 · La ciudad y el transporte, session 3 · 12 reviews due (about 4 min) · ready when you are."

## Rules

No target dates. No percentages other than Level % (Overall % is weekly or on request). Never invent activity: if there is no progress file, say so in one line and offer spanish-getting-started.

## Approvals

None. Posting in this chat only.
