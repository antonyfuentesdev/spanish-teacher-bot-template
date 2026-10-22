---
name: spanish-graduation
description: Use when a learner who has passed the C1 level exam and the C1 one-month check asks for the final exam or graduation, or when spanish-progress-write reports that all graduation gates are met. Runs the C1 graduation oral (presentation with mediation, interview, negotiation) from the Course Pack, scores it with a next-day second pass and records the bot-assessed graduation.
---

# spanish-graduation

## Inputs and access

- C1 graduation oral page (Sets 1, 2, 3): `course/pages/exams/C1-graduation-oral.md` (downloaded from its raw_url in `course/manifest.json`)
- Administration and scoring guide: `course/pages/exams/00-guide.md` (downloaded from its raw_url in `course/manifest.json`)
- Rules: 13.9 Rubrics §4 and §6 (`course/pages/rules/13.9-rubrics.md`; downloaded from its raw_url in `course/manifest.json`); 13.11 Gates §8
- Both pages must be OK in `course/status.md`; otherwise the oral is temporarily unavailable.

## Eligibility (all needed; check from the file and say what is missing)

C1 level exam passed (Group 2) with both scoring passes; C1 minimum lesson hours (75, or 25 if placed at C1); all 12 C1 topics passed or fast-tracked; C1 one-month check passed; no refresher open at any level.

## Sequence

1. Set: first sitting Set 1, then 2, then 3. 20 minutes' preparation, then about 20 minutes.
2. Task 1 presentation with mediation: the bot narrates the set's fixed source (or shows it in chat on the typed track); the learner presents 3–5 minutes and answers the set's questions.
3. Task 2 interview: the set's fixed questions (opinion, hypothesis, defending a view), in order; one follow-up allowed.
4. Task 3 informal negotiation: the bot follows the set's fixed moves and accepts an agreement only under the set's conditions.
5. Test mode throughout. Do not give any result in the call.
6. Score after the call from the transcript: 5 criteria (Task achievement, Coherence and fluency, Range, Accuracy, Interaction) 0–5 per task, one quote per criterion per task. 25 per task, 75 total.
7. Second pass the next day. If totals differ by more than 5 points or pass/fail differs, the lower result stands, flagged.
8. **Pass** (all four): total ≥ 45/75; every task ≥ 13/25; no criterion scored 1; Accuracy ≥ 3 in at least 2 tasks. 60/75 or more = pass with distinction. Pronunciation is never scored.
9. Only after both passes: tell the learner. spanish-progress-write writes the Assessments row (type Graduation, per-criterion scores, task totals, the four conditions yes/no, both totals, Evidence) and the Graduation Milestones row citing it. Overall % reaches 100 only then.
10. Wording: "C1 graduation, bot-assessed". Never an official DELE or SIELE credential.
11. Fail: retake the whole oral at least 21 days later on a new set; at most 3 sittings in 12 months.

## Approvals

None.
