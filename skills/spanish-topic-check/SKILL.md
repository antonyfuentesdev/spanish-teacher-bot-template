---
name: spanish-topic-check
description: Use at the last session of a topic, for a retake, for the optional fast-track at the start of a topic's session 1, or when the learner asks for a quiz on the current topic. Runs the fixed Form A, B, C or D from the topic page in test mode, scores it with evidence and a second pass, and records the result.
---

# spanish-topic-check

Rules: 13.9 Rubrics §2 and §6 (`course/pages/rules/13.9-rubrics.md`; downloaded from its raw_url in `course/manifest.json`) and 13.11 Gates §2–§3 (`course/pages/rules/13.11-progress-tracking.md`; downloaded from its raw_url in `course/manifest.json`). Forms: §9 of the topic's page, `course/pages/topics/<topic-id>.md` (source: that file's tag-pinned `raw_url` in `course/manifest.json`).

## Inputs and access

- The topic page, OK in `course/status.md`. A topic without its four forms cannot be checked: never generate items. If the page is Partial, say the check is temporarily unavailable.
- Assessments rows for this topic (forms used, attempts, dates)

## Which form

- Fast-track (session 1 only, before teaching, once per topic, at most 3 a week; not offered to a self-declared beginner during course A1.1 unless they ask): Form A. Pass = 8/10 or more **and** Part B 4/4; it skips the remaining sessions. Below that, the topic is simply taught; no penalty.
- Regular check: Form A; retakes Form B, then Form C.
- After a failed fast-track: regular check Form B; retakes Form C, then Form D.
- After a third fail: 2 recycle sessions, then one final sitting on Form D if unused; only a learner who has used all four gets the least recently used form, flagged "Reused form".
- Gates: at most 1 topic check a day; never on the same day as the topic's last new-content session; retakes at least 2 days apart; at most 2 retakes.

## Sequence

1. Last-session layout: 10 minutes warm-up review, then the check (about 8 minutes), then feedback and free chat.
2. Say "Ahora viene una prueba corta: sin pistas hasta el final." (A1–A2: "Now it's a short test: no hints until the end.") Test mode: no hints, no correction ladder, no feedback until scoring.
3. Part A: the form's 5 spoken items + 1 listening item (A1–A2 listening: at most 3 short options, read twice), 1 point each, expected answers and accepted variants from the page. Ending-unclear items are re-asked once (at most 2 re-asks), not marked wrong. An answer given only after a hint scores 0.
4. Part B: the form's role play, 4 points: completes the task; uses the main focus correctly at least twice; responds relevantly to two bot turns; understood with no more than one repetition (message, not accent). Use the page's anchors.
5. Say the **provisional** result aloud with 2–3 quotes ("Provisional: 8 of 10. You said «me gustan las uvas»…").
6. Second pass from the transcript and rubric only, before the row is written. Passes differ in pass/fail or by more than 1 point → the lower result stands, flagged "Passes disagree".
7. Pass = 6/10. Feedback after scoring: what went well, what to fix, failed items added to the queue.
8. spanish-progress-write: Assessments row (type Topic check, form, attempt, fast-track yes/no, Part A and B scores, skills S/L/V/G with P blank, total, result, pass-2 total, earliest retake) with an Evidence quote for every point given or withheld; for a fast-track pass, 3 topic items into the queue at step 3; Course up Milestones row if the gate is met.
9. Placement confirmation: if the first two results at a placed level are both under 6/10, **offer** a move down one level; the learner decides. Two topic checks in a row under 40% → offer to go back one course.

## Never

Raise a score on request, skip a gate, reuse a form early, or call a provisional result final.

## Approvals

None.
