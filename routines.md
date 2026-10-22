# Spanish Teacher — routines (build reference only, not shipped)

> **Not shipped.** Templates carry skills, memories and plugins only, so this file never reaches a learner's copy. The shipped, self-contained definitions are in `skills/spanish-getting-started/SKILL.md` (Routine definitions: session reminder and brief, daily tidy-up, weekly report) and `skills/spanish-level-exam/SKILL.md` (One-month check routine). Edit those skills first; keep this file as a mirror for review.

None of these ships live. spanish-getting-started offers them in one card and creates only the ones the learner says yes to, from its own Routine definitions section, using the learner's own times and **timezone** (the IANA zone confirmed during getting started, e.g. America/Mexico_City). The one-month check is created later, by spanish-level-exam, after each level pass. All four keep to the docs' routine fields: owner (this bot), schedule, time zone, input, result, approval and missing-data policy. Runs are idempotent: the same run twice changes nothing.

## 1. Session reminder and brief

- **Schedule:** weekly, on the learner's study days only, 30 minutes before their chosen study time; learner's timezone
- **Created when:** the learner says yes at getting started
- **Prompt:**
  > Run spanish-session-brief. First save any unsaved call or pending check result with spanish-progress-write. Then post one short brief in this chat: today's topic and session, due reviews with minutes, pending retakes, the next gate in plain numbers, level and Level %. End with "Start a voice call when you're ready." If there is no progress file, say so in one line and offer to set up. If today's topic page isn't available, say so and offer a review session. Never invent activity or give a target date.
- **Writes:** only through spanish-progress-write (saving a missed call). **Approval:** none.

## 2. Daily tidy-up

- **Schedule:** daily at a quiet hour, default 11:30 PM; learner's timezone
- **Created when:** the learner says yes at getting started (recommended: it is what saves a lesson if a call wasn't saved)
- **Prompt:**
  > Run spanish-progress-write in reconcile mode. Compare today's voice calls with the Sessions table and write any missing rows, keyed by call start time, never twice. Score any check whose provisional result is in a transcript but not yet recorded, with the second pass. Run the next-day second scoring pass for any level exam or graduation oral from yesterday. Make today's backup if none exists and keep the last 14. Once a week (Mondays), run the spanish-course-setup version check: download only the latest manifest from main (https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json) and compare its version and tag with course/manifest.json; if either differs, run spanish-course-setup (it downloads changed files from the new tag's raw_url and keeps unchanged ones). If any page is Partial in course/status.md, rerun spanish-course-setup once. If a transcript can't be read, ask the learner for a 3-line summary the next time they write and mark the row "reconstructed"; record no score from it. Post nothing if there was nothing to do.
- **Writes:** progress.md, backups/, course/ (setup only). **Approval:** none.

## 3. Weekly report

- **Schedule:** weekly, on the day and hour the learner picks; learner's timezone
- **Created when:** the learner says yes at getting started
- **Prompt:**
  > Run spanish-weekly-report. Reconcile first, then write this week's Weekly log row with the write procedure and post the summary in this chat: level and Level %, Overall % with its one-line meaning, hours this week and to date, lessons planned and met, weeks on plan, results and anything pending, the next gate in numbers and one focus for next week. If there were no lessons, say so in one line and offer a short review. Never round a percentage up and never give a target date.
- **Writes:** progress.md (Weekly log row). **Approval:** none; a Page only if the learner asked for one.

## 4. One-month check reminder (one-time)

- **Schedule:** once, 30 days after the date of a Level up Milestones row, at the learner's usual study time; learner's timezone
- **Created when:** automatically by spanish-level-exam after a level pass (the learner is told when the level is passed); a new one-time routine for each level
- **Prompt:**
  > It's 30 days since the learner passed [level]. Post in this chat: "Time for your 10-minute one-month check on [level]: 10 words and phrases from that level, no hints. It keeps what you learned fresh, and the next level exam needs it. Want to do it now or at your next lesson?" Then run it with spanish-level-exam (one-month check) when the learner says yes. Pass 6/10 → Retention passed; under 6/10 → Refresher due (2 consolidation sessions, then a re-check). The badge is never taken away.
- **Writes:** progress.md through spanish-progress-write. **Approval:** none.
