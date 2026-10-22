---
name: spanish-typed-practice
description: Use after a voice lesson when the learner's typed track is on, when the learner asks to practise reading, writing, spelling or accents in chat, or for the topic's graded writing task. Runs a 5-minute typed block in chat (short text, typed answers, a little dictation) or the 15-minute graded writing task, and records it.
---

# spanish-typed-practice

Off by default. The learner can switch the typed track on or off any time; the Profile records it from that day.

## Inputs and access

- The topic page (`course/pages/topics/<topic-id>.md`; source: that file's tag-pinned `raw_url` in `course/manifest.json`): §10 Typed track, plus today's §3 script, §4 model dialogue and §5 exercise items already used in the lesson
- Voice memos or audio files for dictation, if available; otherwise the learner reads the sentence aloud first and then types it from memory

## Typed block (5 minutes, after a lesson call)

Everything comes from the topic page; never write new texts, questions or sentences.

1. Reading: the §10 short text with its questions, but only once the session outline has taught everything it uses (its numbers, verbs and chunks; for A1.1-1 from session 5 on, because it uses age, numbers and -ar verbs). Before that, show today's §3 script as text and ask its gist questions in writing.
2. Two or three typed replies: §5 exercise items already practised today, answered in writing (personal items with the learner's own facts).
3. A short dictation (1–2 sentences): a line from today's §3 script or the §4 model dialogue, read aloud (or played as a voice memo), then typed: spelling and accents.
4. Feedback: one line per error with the corrected form; accents explained once, briefly.
5. spanish-progress-write: Sessions row type Typed; spelling errors into the Review queue as Error items.

## Graded writing task (once per topic)

- When: in the typed block after the topic's last recycle session before the check (session 9 at A1–A2; the session before the check at higher levels), about 15 minutes.
- Task and model answer: from the topic page. Show the model only after scoring.
- Score: task achievement, organization, range, accuracy, 0–5 each (13.9 §3 bands; `course/pages/rules/13.9-rubrics.md` (downloaded from its raw_url in `course/manifest.json`)), with one quote per criterion.
- Record: Assessments row type Writing task, Result Done, with Evidence. These count toward the typed-track minimum before Group 1 of a level exam (A1 4 · A2 5 · B1 6 · B2 8 · C1 10).

## Approvals

None.
