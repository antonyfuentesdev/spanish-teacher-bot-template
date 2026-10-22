---
name: spanish-placement
description: Use when a new learner says they have studied Spanish before, when spanish-getting-started reaches the placement step, or when a learner asks to be re-placed. Runs the fixed adaptive placement test (about 15 minutes) from the Course Pack, scores it with a second pass, and sets the starting level and topic.
---

# spanish-placement

## Inputs and access

- Placement test page: `course/pages/exams/Placement.md` (downloaded from its raw_url in `course/manifest.json`)
- Rules: 13.9 Rubrics §1, `course/pages/rules/13.9-rubrics.md` (downloaded from its raw_url in `course/manifest.json`)
- The placement page must be OK in `course/status.md`. If not, run spanish-course-setup for it first; if still not OK, do not improvise a test: start the learner at A1.1-1 with an offer to fast-track topics, and say why.

## Sequence

1. Complete beginner path: "Have you studied Spanish before? If not, we can skip the test and start at lesson one." No → skip (spanish-getting-started handles it).
2. Say "Now it's a short test: no hints until the end." Test mode: no hints, no corrections, no feedback until scoring is done. Instructions in English (or the learner's language) until the learner handles step 2.
3. **Speaking ladder** exactly as the placement page: five steps (A1 greetings and personal facts → A2 daily routine → B1 last weekend and changes → B2 plans, wishes and hypotheses → C1 defend an opinion), two prompts each, with the page's fixed rephrases.
   - Struggle = after one rephrase, no relevant answer; or more than half the step's target forms wrong; or mostly English.
   - Stop rule: struggle on a step's first prompt → give its second prompt; two struggles at the same step end the ladder.
4. **Listening:** up to 3 clips at rising speed, each read twice, 2 questions each, as on the page. Play a clip only if the ladder reached that step or the previous clip was understood.
5. **Score:** speaking, listening, vocabulary, grammar 1–5 each (13.9 §1 bands). Pronunciation is noted for feedback only, never scored. Level = the highest step handled comfortably (both prompts relevant, at least half the target forms right, at most one rephrase). None handled comfortably → A1.
6. **Second pass** from the transcript and the rubric only, before anything is recorded. If the two passes disagree, the lower result stands, flagged "Passes disagree".
7. **Result** (wording on the placement page): level, first topic (first course of that level), two strengths, two focus areas, starting Level % and Overall % with their meaning; a skipped level shows as "credited by placement". Explain: "In each topic's first session you can try an optional quick quiz to skip lessons you already know." Also explain before the first fast-track: if the first two results at a placed level are both under 6/10, I'll offer to move down one level; you decide.
8. Hand the facts to spanish-progress-write: Profile (placed level, placed yes/no, level start, current topic), Assessments row type Placement with Evidence (a quote for each prompt and clip), Sessions row type Placement, failed items to the Review queue.

## Rules

- The learner can always ask to start lower. Never place higher than the evidence.
- No target date. Never call the result official; it is "bot-assessed".

## Return

The result in plain words, then the next step (first lesson now or a time).

## Approvals

None.
