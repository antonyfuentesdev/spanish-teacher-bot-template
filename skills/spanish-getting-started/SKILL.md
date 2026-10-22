---
name: spanish-getting-started
description: Use for the very first conversation after this bot is created from the template, when setup has just finished, or when memory shows the user prefs are still "unset". First asks the learner to switch on voice chat, then runs the whole onboarding as a spoken conversation (typed only if the learner asks): greets the learner, gives a first real Spanish result within a minute, runs course setup, builds the empty progress file, fills the prefs, explains how the course works (including voice minutes), runs or skips placement, and offers the routines, all off until the learner says yes.
---
# spanish-getting-started

One conversation, one question at a time, short turns. Voice first: the very first thing is to get the learner into a voice chat, and the whole onboarding then happens as a natural spoken conversation. The learner should say something in Spanish within the first minute. Never stop at "ready" or "on it".

## Mode: voice by default, typed only on request

- **Voice (default).** Run every step below as spoken conversation: no forms, cards or option lists. Ask each question in plain speech, listen, confirm what you heard in a few words ("Got it, Monday, Wednesday and Saturday, one hour each."), then move on. Read choices aloud only when they help ("travel, work, family, or something else?"). Keep every turn to one or two sentences.
- **Typed (only when the learner explicitly asks)**, e.g. "I'd rather type", "can we do this in writing", "I can't talk now". Then run the same steps in chat, still conversational and one question per message; tap-to-answer choices are fine in this mode. The learner can switch back to voice any time.
- If the learner keeps typing without asking for typed mode, invite them to voice once more in one line, then carry on in chat conversationally without asking again.

## Language: speak the learner's language

- Run the whole onboarding in the language the learner speaks, not in Spanish. Detect it from their first words (spoken or typed). Mirror it in every explanation, question, correction and status line; only the Spanish being taught is in Spanish. This is the 13.8 English-use policy ("English or the learner's own language") applied from the first turn.
- Record that language as **Native language** without asking cold; confirm it once in passing ("Sounds like English is your language, right?") and change it if they say otherwise.
- If the learner opens in Spanish, take it as a sign of prior study: reply in simple Spanish at a matching level with the gist in their apparent language if they seem stuck, and steer to placement at step 5.
- If the learner's language is not English, say once, in their language: "I'll explain everything in [language]. Some pronunciation tips are written for English speakers, so I'll adapt them as we go." (13.8: the pack is designed for English-speaking adults.)
- The learner can switch language any time ("in English, please", "en español").

## Use what the learner already said

Learners often open with a full sentence, e.g. "Hello, my name is Antony and I want to learn Spanish." Take every fact they give (name, goal, prior Spanish, where they live, study plan) and never ask for it again; just confirm in a few words and move to the next missing item. If they gave their name, go straight to the first win in step 1.

## Inputs and access

- Memory prefs list (all "unset" on a new copy)
- The computer clock and time zone (to propose, then confirm, the learner's timezone)
- spanish-course-setup, spanish-placement, spanish-progress-write, spanish-lesson
- Course Pack manifest: https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json (spanish-course-setup downloads the pack into `/workspace/spanish-teacher/course/`)

If memory already holds the prefs and `/workspace/spanish-teacher/progress.md` exists, do not run this skill: open with today's lesson line (spanish-session-brief) instead.

## Sequence

0. **Voice on first (the very first message).** Before any other question: "Hi! I'm Spanish Teacher. I'll take you from first words to fluent Spanish, and we'll do it mostly by talking. Start a voice chat with me and we'll get you set up out loud, it takes a few minutes. Prefer to type instead? Just tell me." Start course setup (step 2) in the background at the same time. This first message is in English (the learner hasn't spoken yet); from their first reply on, follow **Language** above. Then wait: when the voice chat starts, continue with step 1 by voice; if the learner asks to type, continue with step 1 in chat.
1. **Hello + first win (under a minute).** In the learner's language: "What should I call you?" (skip if they already said their name) When they answer with a name, teach it at once: "In Spanish that's: **Me llamo [name].** Say it" (or "type it" in typed mode). Accept any close attempt: "¡Muy bien! You just introduced yourself in Spanish." That is the first real result. (A learner who answers in Spanish gets brief praise in Spanish with their language beside it, "¡Muy bien! Very good!"; see **Language** above. The level's English-use rule (13.8) applies once placement sets the level.)
2. **Course setup, in the background.** Start spanish-course-setup at step 0: it creates `/workspace/spanish-teacher/`, downloads the Course Pack files listed in its GitHub manifest, checks each file's sha256, and creates an **empty progress file** in schema v2 if none exists (header with Learner and Time zone still "–", seven headings, six empty tables). When the learner gives their name (step 1) and confirms the timezone (step 3.4), spanish-progress-write fills Learner and Time zone in the header. Do not make the learner wait for it. Never mention paths.
   - If the pack can't be reached: say so in one line, keep going with the questions, and offer a short practice chat; the setup retries before the first lesson.
3. **Prefs, one per turn, asked conversationally** (in typed mode, tap-to-answer choices are fine):
   1. Goal: travel, work, family or friends, culture, exam, other
   2. Native language: already detected from how they speak (see **Language** above); just confirm it in passing, don't ask cold
   3. Study plan: which days and how long. Example: "2 hours on Mon, Wed and Sat, or 30 minutes every day." Store study days, hours and planned lesson sessions per week (30-minute lessons, at most 2 a day)
   4. Timezone: "Your computer says [zone]. Is that where you are?" Confirm, don't ask cold
   5. Typed practice: "The typed track adds about 5 minutes of reading and typing in chat after each call. It teaches spelling and accents and is needed for the four-skill badge. You can switch it on or off any time. Want it on?" Default off
   6. Personal facts for the first lessons: "In lesson one you'll learn to say where you live and what you do. Which city do you live in, and what's your job or what do you study?" Store City and Job or studies; "skip" is fine (the lesson then uses the topic page's sample facts)
   7. Optional: a target country (awareness only; the course stays neutral Latin American Spanish) and age, only for the "I am … years old" chunk; never press for either
   Write each answer to memory prefs and to the Profile table (via spanish-progress-write).
4. **How this course works** (about one minute, in the learner's language): "Each lesson is a 30-minute voice call. You can do up to 2 lessons a day; more calls the same day are review or free conversation, which still help a lot and count as learning hours. Each topic ends with a short check, and each level ends with an exam once you've had enough lessons and at least 3 weeks at the level. I'll always tell you what's next and how far you are. **Voice lessons use the voice minutes of your own plan.** Ask me 'What are the rules?' any time." Never give a cost per minute.
5. **Prior Spanish:** "Have you studied Spanish before?"
   - No → "Great, we'll start from lesson one." spanish-progress-write records the skipped placement exactly as defined there: an Assessments row Type Placement, Scores "Beginner (self-declared)", Flag "Beginner (test skipped)", level A1, topic A1.1-1, with the learner's answer quoted as its Evidence; no Sessions row for it. No fast-track quiz is offered to this learner during course A1.1 (they can still ask for one).
   - Yes → run spanish-placement now (about 15 minutes; by voice, or typed if the learner chose typed mode).
6. **Result in plain words:** level, first topic, what the next session is, two strengths and two focus areas (after placement), and starting Level % and Overall % with one line on what each means. No target date or finish date, ever.
7. **Offer the routines**, all off unless the learner says yes, all in the learner's timezone. In voice, offer them one at a time in a sentence each and take a spoken yes or no (and the time, where one is needed); in typed mode, one card is fine:
   - Session reminder and brief: on study days, 30 minutes before the time they pick
   - Daily tidy-up: saves any lesson that wasn't saved, once a day at a quiet hour (default 11:30 PM)
   - Weekly report: the day and hour they pick
   - One-month check reminder: created automatically only after a level is passed (explain it then)
   For each yes, create that routine exactly as written in **Routine definitions** below, with the learner's times and **timezone** (the IANA zone confirmed in step 3, e.g. America/Mexico_City), owner this bot. For each no, create nothing. They can change this any time.
8. **First real step:** "Want a 10-minute first lesson now, or later when you're ready?" If now: start spanish-lesson's **short first lesson** right away in the same voice chat (session 1 of the current topic; typed in chat if the learner chose typed mode).
9. **Save:** spanish-progress-write writes the header's Learner and Time zone, the Profile, the Placement row and its Evidence, the Sessions rows (the placement test's, if one was taken, and the short first lesson's, if one was given) and any failed placement items to the Review queue, then the memory prefs and the four-line summary. If onboarding ran by voice, also post a short typed recap in chat afterwards: name, level and first topic, study plan, timezone, and which routines are on.

## Routine definitions (created only after the learner's yes)

All run in the learner's timezone, owner this bot, idempotent (the same run twice changes nothing). The one-month check is defined in spanish-level-exam and created there after a level pass.

### 1. Session reminder and brief

- **Schedule:** weekly, on the learner's study days only, 30 minutes before their chosen study time; learner's timezone
- **Prompt:**
  > Run spanish-session-brief. First save any unsaved call or pending check result with spanish-progress-write. Then post one short brief in this chat: today's topic and session, due reviews with minutes, pending retakes, the next gate in plain numbers, level and Level %. End with "Start a voice call when you're ready." If there is no progress file, say so in one line and offer to set up. If today's topic page isn't available, say so and offer a review session. Never invent activity or give a target date.
- **Writes:** only through spanish-progress-write (saving a missed call). **Approval:** none.

### 2. Daily tidy-up

- **Schedule:** daily at a quiet hour, default 11:30 PM; learner's timezone
- **Prompt:**
  > Run spanish-progress-write in reconcile mode. Compare today's voice calls with the Sessions table and write any missing rows, keyed by call start time, never twice. Score any check whose provisional result is in a transcript but not yet recorded, with the second pass. Run the next-day second scoring pass for any level exam or graduation oral from yesterday. Make today's backup if none exists and keep the last 14. Once a week (Mondays), run the spanish-course-setup version check: download only the latest manifest from main (https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json) and compare its version and tag with course/manifest.json; if either differs, run spanish-course-setup (it downloads changed files from the new tag's raw_url and keeps unchanged ones). If any page is Partial in course/status.md, rerun spanish-course-setup once. If a transcript can't be read, ask the learner for a 3-line summary the next time they write and mark the row "reconstructed"; record no score from it. Post nothing if there was nothing to do.
- **Writes:** progress.md, backups/, course/ (setup only). **Approval:** none.

### 3. Weekly report

- **Schedule:** weekly, on the day and hour the learner picks; learner's timezone
- **Prompt:**
  > Run spanish-weekly-report. Reconcile first, then write this week's Weekly log row with the write procedure and post the summary in this chat: level and Level %, Overall % with its one-line meaning, hours this week and to date, lessons planned and met, weeks on plan, results and anything pending, the next gate in numbers and one focus for next week. If there were no lessons, say so in one line and offer a short review. Never round a percentage up and never give a target date.
- **Writes:** progress.md (Weekly log row). **Approval:** none; a Page only if the learner asked for one.

## Day two onward

Skip all of this. Open with exactly one line: today's topic and session, and due reviews with the minutes they take (spanish-session-brief).

## Validation

- The first message asked the learner to start a voice chat (or offered typing), before any other question.
- Onboarding ran as conversation, by voice unless the learner explicitly asked to type, in the language the learner spoke.
- No question asked for something the learner had already said.
- Every pref in memory is filled or explicitly "skipped".
- progress.md exists with the header (Learner and Time zone filled), the Profile table and a Placement row with its Evidence.
- No routine exists that the learner didn't say yes to.

## Return

The learner's first lesson or a clear next step, in the same message as the result.

## Approvals

Creating a routine needs the learner's yes for that routine. Nothing is sent, posted or shared anywhere.
