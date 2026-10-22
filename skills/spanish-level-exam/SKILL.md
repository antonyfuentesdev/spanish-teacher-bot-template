---
name: spanish-level-exam
description: Use when the learner asks for the level test or end-of-level exam, when spanish-progress-write reports that all level-exam gates are met, for a level-exam retake, or for the one-month check 30 days after a level pass. Runs the fixed A1–C1 exam papers from the Course Pack with the exam memory check, scores with evidence and a next-day second pass, and records the result and badge.
---

# spanish-level-exam

## Inputs and access

- Administration and scoring guide: `course/pages/exams/00-guide.md` (downloaded from its raw_url in `course/manifest.json`)
- Exam papers (Forms 1, 2, 3 each):
  - A1: `course/pages/exams/A1-exam.md` (downloaded from its raw_url in `course/manifest.json`)
  - A2: `course/pages/exams/A2-exam.md` (downloaded from its raw_url in `course/manifest.json`)
  - B1: `course/pages/exams/B1-exam.md` (downloaded from its raw_url in `course/manifest.json`)
  - B2: `course/pages/exams/B2-exam.md` (downloaded from its raw_url in `course/manifest.json`)
  - C1: `course/pages/exams/C1-exam.md` (downloaded from its raw_url in `course/manifest.json`)
- Rules: 13.9 Rubrics §3 and §6 (`course/pages/rules/13.9-rubrics.md`; downloaded from its raw_url in `course/manifest.json`); 13.11 Gates §5–§7 (`course/pages/rules/13.11-progress-tracking.md`; downloaded from its raw_url in `course/manifest.json`)
- The guide and the level's paper page must be OK in `course/status.md`. If not, the exam is temporarily unavailable; progress is kept. Never improvise a paper.

## Eligibility (state the gate status before offering)

All needed: every topic of the level passed or fast-tracked; all consolidation sessions done; minimum lesson hours (A1 24, A2 24, B1 57, B2 66, C1 75; at a placed level 8, 8, 19, 22, 25); at least 3 weeks since Level start; the previous level's one-month check passed; no refresher open. Say what is missing in plain numbers: "You can take the A2 exam after 24 lesson hours; you have 18." (Spanish from B1.)

## Structure

- **Group 2 (everyone):** Listening (25 items) + Speaking (3 tasks: monologue, conversation, role play; 5 criteria × 0–5) + the **exam memory check**: 10 items of this level created at least 21 days earlier (queue or archive), drawn and written into the Evidence block before the sitting; 7/10 needed.
- **Group 1 (typed track only):** Reading (25 items, texts in chat) + Writing (dictation 5 points, minus 1 per two spelling or accent errors; tasks 20 points). Only after the typed-track minimum of graded writing tasks at this level: A1 4 · A2 5 · B1 6 · B2 8 · C1 10.
- One paper group per day at most; Group 2 is one sitting, Group 1 another day. Tell the learner the length first (timings table on the guide).

## Sequence

1. Read the guide's "what the learner is told" text (English at A1–A2, Spanish from B1).
2. Form: first sitting Form 1, then Form 2, then Form 3 (for each group separately).
3. "Ahora viene el examen: sin pistas hasta el final." (A1–A2: "Now it's a short test: no hints until the end.") Test mode: no hints, no correction, no feedback until scoring. A1–A2 listening: at most 3 short options, read twice.
4. Run the papers exactly as written: scripts, fixed questions, fixed role-play turns, answer keys, anchors.
5. Score after the call from the transcript, one evidence quote per item (listening, reading) and per criterion (speaking, writing). Do not announce a pass yet: say "I'll score it carefully and give you the result tomorrow."
6. Second pass the next day (daily tidy-up runs it) from transcript and rubric only. If the passes differ in pass/fail or by more than 5 points per group, the lower result stands, flagged "Passes disagree".
7. **Pass rule:** listening + speaking ≥ 30/50 **and** exam memory check ≥ 7/10 moves the learner up. Reading + writing ≥ 30/50 only decides the four-skill badge; failing or skipping it never blocks a level up.
8. Badge wording: "Spoken Spanish [level] — listening and speaking (bot-assessed)" or "Spanish [level] — listening, speaking, reading and writing (bot-assessed)". Never DELE, SIELE or an official certificate.
9. spanish-progress-write: Assessments row type Level exam (form, attempt, every paper score, retention set, group totals, result, pass-2 total, Evidence); on a pass, Level up and Badge Milestones rows citing it; new Level start; then create the one-time **one-month check** routine for 30 days later (definition in **One-month check routine** below) and tell the learner about it.
10. Fail: targeted review from the error log; retake only the failed group, at least 10 days later, on the next form; at most 3 sittings, then a 4-week review cycle.

## One-month check (30 days after a level pass)

10 minutes, 10 items from that level (queue and archive), same out-loud prompts as review, no hints. Pass 6/10 → Retention passed. Under 6/10 → Refresher due: 2 consolidation sessions, then a re-check; the next level exam waits. The badge is never taken away. Record as Assessments type Retention check, form "one-month".

## One-month check routine (one-time, created at step 9)

Owner this bot; learner's timezone; a new one-time routine for each level passed; idempotent.

- **Schedule:** once, 30 days after the date of a Level up Milestones row, at the learner's usual study time; learner's timezone
- **Prompt:**
  > It's 30 days since the learner passed [level]. Post in this chat: "Time for your 10-minute one-month check on [level]: 10 words and phrases from that level, no hints. It keeps what you learned fresh, and the next level exam needs it. Want to do it now or at your next lesson?" Then run it with spanish-level-exam (one-month check) when the learner says yes. Pass 6/10 → Retention passed; under 6/10 → Refresher due (2 consolidation sessions, then a re-check). The badge is never taken away.
- **Writes:** progress.md through spanish-progress-write. **Approval:** none.

## Approvals

None.
