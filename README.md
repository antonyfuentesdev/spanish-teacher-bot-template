# Spanish Teacher — Grok Bot template

Source files for the **Spanish Teacher** Grok Bot template, by Antony Fuentes. This repo is the template's source of truth: the bot's identity, starting memories, routines and 13 skills. The Marketplace listing is exported from the Spanish Teacher bot itself and stays a private draft until reviewed. Changes go through pull requests to the protected `main` branch.

Licence: CC BY-NC 4.0 (see LICENSE). Attribution: "Spanish Teacher bot template by Antony Fuentes". Commercial use requires the author's permission.


The bot is a general-purpose, voice-first Spanish tutor bot (CEFR A1 to C1, neutral Latin American Spanish). Not yet published to the Grok Bot Marketplace. The course content itself is not in this repo: it is the public **Spanish Teacher Course Pack** on GitHub (current release: `version` and `tag` in the manifest), downloaded and checked at setup. The repo is the pack's only home (licence CC BY-NC 4.0, see LICENSE there).

- Repo: https://github.com/antonyfuentesdev/spanish-teacher-course-pack
- Manifest (what the bot reads, always from `main`: version, tag, and each file's repo path, tag-pinned raw_url and sha256): https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json

## What's in this repo

| File | What it is |
| --- | --- |
| `identity.json` | Bot name, title, the 3-sentence description (with the voice-minutes line), avatar note, gettingStarted skill name, pack links, learner folder |
| `memories.md` | The 9 memory lines shipped with the template: contract, job, prefs list (all unset), files-not-memory, course content (manifest URL), fixed values, language rule, integrity, privacy |
| `skills/spanish-getting-started/SKILL.md` | First conversation: first Spanish result within a minute, background course setup, empty progress file, prefs, how the course works and voice minutes, placement or beginner start, routines offer (off until yes) with the full definitions of the session reminder and brief, daily tidy-up and weekly report, first lesson |
| `skills/spanish-course-setup/SKILL.md` | Creates /workspace/spanish-teacher/, reads `main/manifest.json`, downloads the 72 page files from each tag-pinned `raw_url` into `course/` (repo paths), verifies each sha256 with one retry, marks failures Partial, writes status.md, creates the empty progress file; includes the setup script and the full file list |
| `skills/spanish-placement/SKILL.md` | Fixed adaptive placement (speaking ladder, stop rule, listening clips), 1–5 skill bands, second pass, result wording |
| `skills/spanish-lesson/SKILL.md` | One 30-minute lesson from the topic page: daily cap, fast-track offer, 8-step sequence, which script and exercises go in which session, correction ladder, ending check, English-use policy, the 10-minute short first lesson, the typed lesson fallback, chat summary, save |
| `skills/spanish-review/SKILL.md` | Out-loud spaced review: due order, grading, step intervals on study days, overdue rule, short 15-minute session |
| `skills/spanish-topic-check/SKILL.md` | Fixed Forms A–D in test mode, form order, fast-track rule, 10-point scoring, provisional result, second pass, evidence |
| `skills/spanish-level-exam/SKILL.md` | A1–C1 papers, eligibility gates, Group 2 / Group 1 pass rule, exam memory check, next-day second pass, bot-assessed badges, retakes, one-month check and its one-time routine definition |
| `skills/spanish-graduation/SKILL.md` | C1 graduation oral: three tasks, 75-point rule with four pass conditions, next-day second pass, bot-assessed graduation |
| `skills/spanish-progress-write/SKILL.md` | The only writer of progress.md: schema v2 tables, safe write procedure, hours, Level % and Overall % with caps, all gates, pending results, reconcile mode |
| `skills/spanish-session-brief/SKILL.md` | Pre-session brief and the one-line day-two opening |
| `skills/spanish-weekly-report/SKILL.md` | Weekly log row and chat summary with Overall % and its meaning |
| `skills/spanish-typed-practice/SKILL.md` | Optional 5-minute typed block (built only from the topic page) and the graded writing task |
| `skills/spanish-progress-report/SKILL.md` | "How am I doing", rules, exam timing, "how long will it take" answered with Level %, Overall % and what is left in the level (never a time estimate), disputes without changing records |
| `routines.md` | Build reference only, **not shipped**: a mirror of the 4 routine prompts and schedules. The shipped definitions are inside spanish-getting-started (session reminder and brief, daily tidy-up, weekly report) and spanish-level-exam (one-time one-month check) |

## Install order (when the bot is built)

1. Create the bot from `identity.json`: name, title, avatar, description.
2. Add the 9 memories from `memories.md`, one per line.
3. Add the 13 skills from `skills/`, starting with spanish-progress-write and spanish-course-setup (others call them), then the rest; spanish-getting-started last.
4. Set gettingStarted to `spanish-getting-started`.
5. Do not create any routine. spanish-getting-started and spanish-level-exam carry the routine definitions themselves and create routines only after the learner says yes; `routines.md` is not shipped.
6. Run a typed check on a fresh copy: getting started as a complete beginner and as a learner who has studied before; delete `course/` and confirm spanish-course-setup restores all 72 files with matching sha256; check that a second progress write of the same call changes nothing.
7. Only then make the template. Templates carry skills, memories and plugins; the learner's files, routines and history are never copied.

## Course Pack source

The bot reads the pack only from GitHub raw files; GitHub is the pack's only home (an earlier Notion copy was retired on Oct 22, 2026 because public Notion pages render only their first ~20 blocks without a browser). It always reads the latest manifest from `main` for the version and release tag, then downloads each file from the manifest's own `raw_url`, which is pinned to that tag (e.g. `.../spanish-teacher-course-pack/<tag>/pages/topics/A1.2-4.md`); it never builds links from `main`. Every file is checked against its SHA-256. Skills refer to pages by their local path, `course/<repo path>`. The daily tidy-up routine runs the version check every Monday (`version` and `tag` from `main/manifest.json`), and spanish-session-brief and spanish-lesson also run it at session start when the last check is more than 7 days old (so learners without routines still get updates); setup reruns when either changes. Nothing here needs a sign-in, connector or token.
