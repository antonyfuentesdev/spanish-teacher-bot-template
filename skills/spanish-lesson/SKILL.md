---
name: spanish-lesson
description: Use when the learner wants a lesson, starts a voice call to study, says "let's continue", or accepts the session brief. Teaches one 30-minute lesson session of the current topic from its Course Pack topic page (or a consolidation session, the 10-minute short first lesson, or a typed lesson in chat when voice isn't used), with the hint-first correction ladder and the English-use policy, then saves it.
---

# spanish-lesson

## Inputs and access

- `/workspace/spanish-teacher/progress.md`: Profile (current course · topic · session, typed track), Review queue (due items), Assessments (pending retakes). If the file can't be read during a live call, use the latest session brief in chat and confirm: "¿Seguimos con [tema], sesión [n]?"
- The current topic page from the Course Pack, saved at `course/pages/topics/<topic-id>.md` (e.g. `course/pages/topics/A1.2-4.md`; source: that file's tag-pinned `raw_url` in `course/manifest.json`; every file is listed in `course/manifest.json`). It must be OK in `course/status.md`; if not, run spanish-course-setup for it, and if it is still not OK, say this topic is temporarily unavailable and offer review or free talk (counted as learning hours, not lesson hours).
- Rules: 13.2 Topic format (`course/pages/rules/13.2-topic-template.md`; downloaded from its raw_url in `course/manifest.json`), 13.8 Teaching notes (`course/pages/rules/13.8-teaching-notes.md`; downloaded from its raw_url in `course/manifest.json`), 13.10 Spaced review (`course/pages/rules/13.10-spaced-review-schedule.md`; downloaded from its raw_url in `course/manifest.json`), and for session 1 of some topics the worked examples 13.3–13.7 (`course/pages/rules/13.3-…` to `13.7-…`, listed in `course/manifest.json`).

## Before the lesson

1. Check the daily cap: at most 2 new-content lesson sessions per calendar day (a short first lesson and a typed lesson each count as one). A third call that day is Review (spanish-review) or Free talk; say so kindly.
   - If `course/last-version-check.txt` is missing or more than 7 days old, run the spanish-course-setup version check first (a few seconds); if an update is needed, teach today's lesson from the cached files and run setup after it.
2. If a previous call or check is unsaved, run spanish-progress-write first and open with its result ("Your topic 2 check is saved: 8 of 10, passed.").
3. Session 1 of a topic: offer the optional fast-track **before any teaching** (at most 3 a week; Form A; spanish-topic-check). Saying no is fine. Not offered to a self-declared beginner (Placement Scores "Beginner (self-declared)") during course A1.1, unless they ask.
4. Last session of a topic: run spanish-topic-check instead of a lesson.

## The 30-minute sequence (take the content for this session number from the topic page's session outline, scripts, exercise bank, task cards, drill and seeds)

| Step | Min | What happens |
| --- | --- | --- |
| 0 Opening | 1 | Greeting, today's goal, announce the final speaking task |
| 1 Input | 4 (B2–C1: 6; A1: 5) | Script from the page, played twice; 2–3 gist questions. Announce each speaker ("Habla el mesero:") |
| 2 Noticing | 3 | Point to one form; learner spots the pattern; rule in 1–2 sentences |
| 3 Practice | 7 (C1: 5) | 6–8 items from the exercise bank, controlled then personal |
| 4 Speaking task | 8 (A1: 7; C1: 10) | Task card adapted to the learner's goal |
| 5 Pronunciation | 3 | The topic's drill; scored 0–2, recorded only; the learner hears "Pronunciation practice: done" |
| 6 Spaced review | 3 | Up to 8 due items out loud (13.10) |
| 7 Close | 1 | 3-line spoken summary |

- New active items: 4–5 per lesson session at A1–A2, 5–6 at B1–C1, never more than 10 a day; 2 per session while review is overdue.
- A logged error with 3 misses → swap 2 minutes of practice for a 2-minute mini-lesson on it.
- Learner talk time: at least 12 of 30 minutes.
- Personal facts come from the Profile: when a topic teaches talking about oneself, teach the learner's real facts as chunks.
- Personal facts come from the Profile (Age, City, Job or studies). If one is "–" or skipped, ask once when the topic needs it ("Which city do you live in?") and save it; if they'd rather not say, use the page's sample fact (e.g. "Vivo en Heredia.").
- Consolidation sessions (2 per course, before the course's last topic check): the extended role play listed for that course in 13.1.

## Which materials in which session

The topic page's §2 Session outline gives each session's focus, speaking task and new items. If the page has a "Materials per session" line under §2, follow it exactly. Otherwise:

- **Input (§3 scripts):** the script whose content shows the session's focus; when none clearly does, Script 1 in session 1, Script 2 in session 2, Script 3 in session 3, then the same order again. A replayed script is announced as "listen again" and keeps its own gist questions; recycle sessions replay the script the learner found hardest.
- **Practice (§5 exercise bank):** 6–8 items that practise this session's focus and new items, in bank order, starting with ones not yet used; if the focus has fewer than 6 items, fill with earlier items, logged-error items first. Recycle sessions mix items from earlier sessions, logged errors first.
- **Speaking task:** the outline's task for the session, with §6 task cards where the outline names them; §4 model dialogue as the target in the session that first teaches all its lines and in recycle sessions.
- Never write new scripts, items or questions: if the page has too little for a step, shorten that step and give the time to review.

## Correction and language

- **Hint-first ladder** (practice step): 1 signal + prompt ("Almost! One word…"), 2 clue ("So: me gust…?"), 3 model and repeat. At most 2 rungs, then the model. Praise the fix. Log every corrected error.
- **During the speaking task:** only a quick recast when meaning is blocked; everything else in the feedback afterwards.
- **Ending check:** when the form depends on an unstressed ending or stress (llame / llama, hablo / habló), confirm what was said before correcting; if unclear, log "ending unclear", never a grammar error.
- **English use:** A1 all rules, instructions and hints in English (or the learner's language); the Spanish being taught, the scripts and their gist questions stay in Spanish, each gist question followed by its English meaning the first time ("¿Cómo se llama ella? — What's her name?"), and answers in English are accepted for gist; A2 rules and hints in English, simple instructions in Spanish; B1 and up in Spanish.
- Neutral Latin American Spanish; regional words only labelled with their countries.
- Pronunciation is coaching only: never part of any pass decision.

## Short first lesson (10 minutes, getting started only)

Offered once, right after getting started. Session 1 of the current topic, cut down: 0 Opening (1 min) · 1 Input (3 min: the session's script, played twice, 2 gist questions) · 3 Practice (5 min: the first 4–5 of the session's items, including the learner's real name and facts) · 7 Close (1 min). No fast-track offer, no pronunciation step, no review (nothing is due yet). Save it as a Sessions row type Lesson, Session 1, the real minutes, Notes "short first lesson"; it counts as one lesson session toward the daily cap and its minutes count as lesson hours. The session number does not advance: the next lesson is the full session 1, which then counts as session 1 done. New items it introduced go into the Review queue at step 1.

## Typed lesson (when voice isn't used)

If the learner asks to study in chat, can't use voice right now, or a voice call fails, run the same session in chat with the same materials: the script shown as text once (A1–A2: line by line, speakers labelled), gist questions typed; noticing as in voice; practice items typed one at a time with the same hint ladder; the speaking task as a typed role play; spaced review typed. Skip the pronunciation step (Drill "–") and the ending check (spelling is visible). Save it as a Sessions row type Lesson, Notes "typed lesson", real minutes up to 30: it counts as a lesson session (cap and lesson hours) exactly like a voice lesson and advances the session number. No extra typed block afterwards; spelling errors go into the Review queue as Error items.

## After the call

1. Post the full summary in chat (A1–A2 headings in English with the Spanish beside them; Spanish from B1): Today (course · topic · session n of N · minutes), Your level (level · topics done x of y · lesson hours at this level x of minimum · Level %), New, Well done, To improve, Pronunciation practice done, Review (items due next), Next (session and day).
2. Typed track on → spanish-typed-practice (5 minutes in chat).
3. spanish-progress-write: Sessions row (type Lesson or Consolidation, new items, reviewed, drill score, top errors), new items into the Review queue at step 1, reviewed items moved by the grading rules, Profile session number +1.
4. Milestone messages once each (first lesson, first topic passed with its can-do line, course up, 10/25/50/100/200 learning hours, 4 weeks on plan, level up), written as Encouragement rows whose Based on cites the Sessions IDs (or Assessments IDs) that triggered them.

## Never

Invent content not on the page; give a target date; say a level is passed before its Milestones row exists.

## Approvals

None.
