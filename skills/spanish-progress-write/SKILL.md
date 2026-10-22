---
name: spanish-progress-write
description: Use after every voice call or typed session, when a check result is pending, when the daily tidy-up routine reconciles missed calls, or whenever any other spanish- skill needs to read or change the learner record. Reads, updates and safely writes /workspace/spanish-teacher/progress.md (schema v2), computes Level % and Overall %, checks gates, and updates the four-line memory summary.
---

# spanish-progress-write

The only skill that writes the learner record. Every other skill hands it the facts (from the call transcript) and it writes them. Full rules: pack page 13.11 Progress tracking (`course/pages/rules/13.11-progress-tracking.md`; downloaded from its raw_url in `course/manifest.json`). If this skill and 13.11 ever differ, 13.11 wins.

## Inputs and access

- `/workspace/spanish-teacher/progress.md` (source of truth), `progress-archive.md`, `backups/`
- The call transcript (voice-call transcript card) or the chat for typed work
- Mode: **write** (after a call), **reconcile** (daily tidy-up: find calls with no Sessions row), or **create** (first run: empty file)

## File format (schema v2)

Header, then six tables in this order, each directly under its own heading line (`## Profile`, `## Sessions`, `## Assessments`, `## Evidence` (blocks only, right after the Assessments table), `## Review queue`, `## Milestones`, `## Weekly log`). Dates and times are ISO 8601 with the learner's time-zone offset. One field per cell; a dash (–) means empty. IDs never change and are never reused.

```
# Spanish Teacher progress
Schema: v2 · Learner: <name> · Time zone: <IANA zone> · Created: <ISO time>
```

- **Create mode** (spanish-course-setup, before the learner has answered anything): header with `Learner: –` and `Time zone: –`, the seven headings, six empty tables (header rows only). spanish-getting-started then fills Learner and Time zone in place once the learner confirms them; until then every time is written with the computer clock's offset.

1. **Profile** (one row per field; updated in place): `| Field | Value |` with the fields Name, Native language, Goal, Target country (awareness only), Study plan, Planned lesson sessions per week, Typed track, Age (for the "I am … years old" chunk; only if the learner offers it), City (for the "Vivo en…" chunk; optional), Job or studies (for the "Soy… / Trabajo en…" chunk; optional), First session, Placed level, Placed (skipped levels), Current level, Level start, Current course · topic · session, Lowered active targets
2. **Sessions**: `| ID | Start | Minutes | Type | Level | Topic | Session | New items | Reviewed | Drill | Assessment IDs | Top errors | Next review | Level % | Overall % | Notes |` — IDs S-0001…; Type = Placement, Lesson, Consolidation, Review, Typed, Exam, Retention check, Free talk, Outside
3. **Assessments**: `| ID | Date | Session ID | Type | Level | Topic | Form | Attempt | Fast-track | Scores | Skills S/L/V/G/P | Total | Result | Pass 2 total | Flag | Earliest retake | Evidence |` — IDs A-0001…; Type = Placement, Topic check, Level exam, Retention check, Writing task, Graduation; Result = Placed, Pass, Fail, Pass with distinction (graduation only), Refresher due, Done (writing task); Flag = –, Passes disagree, Learner dispute, Re-sit (transcript lost), Reused form, Beginner (test skipped). P (pronunciation) is always blank.
   - **Evidence** section after the table: one block per assessment, `### E-A-0002` then one line per item: `- A3 (0/1): "Me gusta los tomates" (expected gustan)`
   - **Skipped placement** (a learner who says they have never studied Spanish): one Assessments row with Session ID –, Type Placement, Level A1, Topic A1.1-1, Form –, Attempt 1, Fast-track No, Scores "Beginner (self-declared)", Skills –, Total –, Result Placed, Flag "Beginner (test skipped)". Its Evidence block quotes the learner's answer: `- Prior Spanish: "No, never" (self-declared beginner; no test)`. No Sessions row is written for it (no test was taken); the first lesson gets its own row.
4. **Review queue**: `| ID | Item | Type | Level | Topic | Created | Step | Due | Last result | Miss count |` — IDs Q-0001…; Type = Word, Chunk, Frame, Irregular, Sound, Error; Step 1–5; Last result = Correct, Hint, Missed. Items reaching Done move to `progress-archive.md` (same columns plus Retired date)
5. **Milestones**: `| ID | Date | Type | Level | Based on | Gate check | Detail |` — IDs M-0001…; Type = Course up, Level up, Badge, Retention passed, Refresher due, Move down, Graduation, Encouragement. Based on must list existing Assessments IDs; Encouragement rows list the Sessions IDs (or Assessments IDs) that triggered them. A row without them is invalid
6. **Weekly log**: `| Week | Planned lessons | Lessons met | Lesson hours | Learning hours | Learning hours to date | Weeks on plan | Level | Topics done | Writing tasks | Level % | Overall % | Assessments | Pending | Gate status | Focus next week |`

## Write procedure (every write)

1. Read the whole file; check the title line is `# Spanish Teacher progress` and the schema line says `v2`.
2. Build the new version; write it to `progress.tmp.md`.
3. Validate: title and schema line present; the seven headings in order and the six table headers exactly as above; IDs unique and in order; no table has fewer rows than before (except Review queue rows moved to the archive); every Milestones row cites existing IDs (Assessments IDs; for Encouragement rows Sessions or Assessments IDs); every Assessments row has an Evidence block (for a skipped placement, the learner's answer).
4. If no backup exists for today, copy `progress.md` to `backups/progress-YYYY-MM-DD.md`; delete backups older than the last 14.
5. Replace `progress.md` with the temporary file. If validation fails, keep the old file, write nothing, and tell the learner in chat that the update needs a retry.
6. No duplicates: a Sessions row is keyed by the call's start time; if that key exists, skip it. Assessments are keyed by Session ID + Type + Form.
7. Rows are added, not overwritten, except the header's Learner and Time zone, Profile rows and Review queue rows. A correction is a new row with a note.
8. Update memory: the four-line summary (level, current topic, top 3 errors (Error items with the highest miss count), next review date) and any changed pref from memory line 3. Nothing else from the file goes to memory.

## Hours

- **Lesson hours** (used by gates) = Lesson + Consolidation minutes at the current level. At a placed level (placed above A1) they also include Typed minutes, check and fast-track time, and up to 1 Review session (30 min) a day.
- At most 2 new-content lesson sessions count per calendar day; extra calls that day are Review or Free talk.
- **Learning hours** = all minutes of every type, including Outside rows the learner reports in chat.

## Percentages (never rounded up, estimated or raised on request)

- **Level %** = round down (45 × T + 30 × H + 10 × C + 15 × E)
  - T = topics at this level passed or fast-tracked ÷ topics in the level (A1, A2: 6; B1, B2, C1: 12)
  - H = lesson hours at this level ÷ the level's minimum lesson hours, capped at 1
  - C = consolidation sessions done ÷ required (2 per course)
  - E = 1 only when the Level up Milestones row exists. So Level % stops at 85% until the level exam is passed, the second scoring pass agrees and the Level up row is written
- **Overall %** = round down (95 × (plan hours of completed levels + plan hours of current level × Level % ÷ 100) ÷ 328 + 5 × G)
  - Plan hours: A1 32, A2 32, B1 76, B2 88, C1 100 (total 328; source: the hours in course/pages/rules/13.1-syllabus.md, which win if they ever differ). A level skipped by placement counts as completed ("credited by placement")
  - G = 1 only when the Graduation Milestones row exists. So Overall % stops at 95% until graduation

## Gates (check from the file; write the result in the Milestones "Gate check" cell)

| Level | Minimum lesson hours before the level exam | If placed at this level |
| --- | --- | --- |
| A1 | 24 h (48 sessions) | 8 h (A1 placement follows the full minimum) |
| A2 | 24 h (48) | 8 h (16) |
| B1 | 57 h (114) | 19 h (38) |
| B2 | 66 h (132) | 22 h (44) |
| C1 | 75 h (150) | 25 h (50) |

- Caps: at most 2 lesson sessions a day; 1 topic check a day; 1 exam paper group a day; 3 fast-tracks a week.
- **Course up:** all 3 topics passed or fast-tracked and both consolidation sessions done.
- **Level exam eligibility:** all topics of the level passed or fast-tracked; all consolidation sessions done; minimum lesson hours met; at least 3 weeks since Level start; the previous level's one-month check passed; the level's exam page is OK in the course cache.
- **After each level pass:** a one-month check 30 days later (10 items, pass 6/10). Under 6/10: Refresher due (2 consolidation sessions, then a re-check); the next level exam waits. The badge is never taken away.
- **Graduation:** C1 exam passed with both scoring passes; C1 minimum lesson hours and all 12 C1 topics; C1 one-month check passed; no refresher open; graduation oral passed with both passes.
- **Never early:** never say a level is passed or the course is finished until the Milestones row is written after the second scoring pass. A learner's request can never create, raise or skip an Assessments or Milestones row, or change a percentage.

## Pending results

- Every check's provisional result is said aloud in the call, with 2–3 quotes, so the transcript holds it.
- Whichever runs first (this skill after the call, the daily tidy-up, or the next session's opening) scores it with the second pass and writes the row, dated the call's date.
- No transcript within 7 days: record nothing from memory; offer a re-sit on the next form (not a retake, no wait), flagged "Re-sit (transcript lost)". Keep the lesson time as a Sessions row with Notes "reconstructed, no transcript".

## Reconcile mode (daily tidy-up)

Compare the voice calls since the last Sessions row with the file; write any missing rows (never twice); run second scoring passes that are due (level exams, graduation oral, next day); make today's backup if none exists. If a transcript can't be read, ask the learner in chat for a 3-line summary next time they write and mark the row "reconstructed"; record no assessment from it.

## Return

To the learner, after a call: the chat summary from spanish-lesson (level, topics done, lesson hours at this level, Level %). Never file paths. If a write failed: one line saying it will be retried.

## Approvals

None: writes only inside `/workspace/spanish-teacher/`.
