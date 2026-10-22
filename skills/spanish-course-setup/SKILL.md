---
name: spanish-course-setup
description: Use on first run (called by spanish-getting-started), when the course files in /workspace/spanish-teacher/course/ are missing or fail their check, when the weekly tidy-up finds a new Course Pack version in the manifest, or when the learner says course content seems missing. Creates the learner folder, downloads the Course Pack from its public GitHub manifest, verifies every file's sha256 and creates an empty progress file if none exists.
---

# spanish-course-setup

Builds and checks everything the course needs on this computer. Safe to rerun: it never overwrites the progress file, skips files that already match the manifest, and replaces a file only after the new copy passes its check.

## Inputs and access

- **Manifest (the only entry point, always read from `main`):** https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json
  - `version`, `tag` (the release tag, e.g. `v1.3`), `manifest_latest`, `date`, `raw_base`, `page_count`, and `pages`: for every page its `title`, `section`, `file` (repo path), `raw_url` and full `sha256`
  - Every `raw_url` is pinned to the release tag (`.../spanish-teacher-course-pack/<tag>/<file>`), so a learner's files never change underneath them between versions. **Download each file from its own `raw_url` exactly as the manifest gives it; never build a `.../main/<file>` link yourself.** A page with no `raw_url` is Partial.
  - Repo (public, no sign-in): https://github.com/antonyfuentesdev/spanish-teacher-course-pack
- The pack has 72 page files; with the manifest that is 73 downloads. Releases up to v1.2 also list one entry with `file: null` (a retired landing page for people); it has nothing to download and is skipped.
- Tools: the shell (Python 3 standard library, no installs) and the file tools for `/workspace/spanish-teacher/`. No connector, account, cookie or token. The pack's only home is the GitHub repo; there is no other copy to read.

## Folder layout

```
/workspace/spanish-teacher/
  progress.md            the learner record (schema v2; spanish-progress-write)
  progress-archive.md    retired review items
  backups/               daily copies of progress.md, last 14 kept
  course/
    manifest.json        the manifest as downloaded (its version and tag = cached version)
    status.md            version and tag, date checked, counts, one line per page: OK or Partial
    last-version-check.txt  date and time of the last version check (full run or --check-version)
    pages/...            every page file at its repo path, e.g. course/pages/topics/A1.2-4.md
    containers/...       the three section lists (rules, topics, exams)
```

Other skills refer to pages by these paths (`course/<repo path>`). Only this bot writes these names. Never show paths to the learner.

## Sequence

1. Create `/workspace/spanish-teacher/`, `backups/` and `course/` if missing.
2. Save the script below as `/workspace/spanish-teacher/course_setup.py` (overwrite each time so it matches this skill) and run `python3 /workspace/spanish-teacher/course_setup.py`. It:
   1. downloads the latest manifest from `main` (one retry after 2 seconds) and reads its `version` and `tag`;
   2. for every page with a `file`: skips it if the local file already matches its `sha256`; otherwise downloads the page's own tag-pinned `raw_url` from the manifest, checks the sha256, retries once on a network error or mismatch, and writes the file through a `.tmp` copy so a good old file is never replaced by a bad new one;
   3. marks a page **Partial** only if it is still missing or mismatched after the retry;
   4. saves `course/manifest.json` and writes `course/status.md`;
   5. prints one JSON line: version, tag, pages, files_ok, partial.
3. If `progress.md` does not exist, create it in spanish-progress-write's **create mode**: title `# Spanish Teacher progress`, schema line with `Learner: –` and `Time zone: –` (spanish-getting-started fills them once confirmed), the seven headings and six empty tables. Never overwrite an existing file.
4. Return the result (below).

Run it in the background while the learner is already talking: getting started never waits for it (it takes about 15 seconds). Before a lesson, check, exam or placement, its own files must be OK in `status.md` (topic page; placement page; level exam page plus `pages/exams/00-guide.md`; graduation oral page); if one is Partial, rerun this skill once first.

```python
#!/usr/bin/env python3
"""Spanish Teacher course setup: download the Course Pack listed in its GitHub manifest and verify it.
Usage: python3 course_setup.py [target_dir]            (default /workspace/spanish-teacher/course)
       python3 course_setup.py --check-version [dir]   (weekly check: compare version/tag only, download nothing)"""
import hashlib, json, os, sys, time, urllib.request
MANIFEST = "https://raw.githubusercontent.com/antonyfuentesdev/spanish-teacher-course-pack/main/manifest.json"
args = [a for a in sys.argv[1:] if not a.startswith("--")]
check_only = "--check-version" in sys.argv
target = args[0] if args else "/workspace/spanish-teacher/course"

def get(url):
    with urllib.request.urlopen(url, timeout=30) as r:
        return r.read()

def fetch(url, sha=None, tries=2):
    err = ""
    for i in range(tries):
        try:
            data = get(url)
            if sha is None or hashlib.sha256(data).hexdigest() == sha:
                return data, "OK"
            err = "sha256 mismatch"
        except Exception as e:
            err = f"{type(e).__name__}: {e}"
        if i + 1 < tries:
            time.sleep(2)
    return None, err

def sha_of(path):
    return hashlib.sha256(open(path, "rb").read()).hexdigest()

raw, err = fetch(MANIFEST)          # always the latest manifest on main
if raw is None:
    print(json.dumps({"manifest": "FAILED", "error": err})); sys.exit(1)
m = json.loads(raw)
ver, tag = m.get("version"), m.get("tag")
mp = os.path.join(target, "manifest.json")
old = {}
if os.path.exists(mp):
    try: old = json.load(open(mp))
    except Exception: old = {}
same = (old.get("version"), old.get("tag")) == (ver, tag)

stamp = os.path.join(target, "last-version-check.txt")
def mark():
    os.makedirs(target, exist_ok=True)
    open(stamp, "w").write(time.strftime('%Y-%m-%dT%H:%M%z') + "\n")

if check_only:
    mark()
    print(json.dumps({"latest_version": ver, "latest_tag": tag, "cached_version": old.get("version"),
                      "cached_tag": old.get("tag"), "update_needed": not same})); sys.exit(0)

os.makedirs(target, exist_ok=True)
rows, ok, partial = [], 0, 0
for p in m["pages"]:
    f = p.get("file")
    if not f:
        rows.append((p["title"], "-", "No file (landing page, releases up to v1.2)")); continue
    dest = os.path.join(target, f)
    if os.path.exists(dest) and sha_of(dest) == p["sha256"]:
        ok += 1; rows.append((p["title"], f, "OK")); continue
    url = p.get("raw_url")              # always the manifest's own (tag-pinned) link; never built from main
    if not url:
        partial += 1; rows.append((p["title"], f, "Partial (no raw_url in manifest)")); continue
    data, st = fetch(url, p["sha256"])  # one retry inside fetch
    if data is None:
        partial += 1; rows.append((p["title"], f, f"Partial ({st})")); continue
    os.makedirs(os.path.dirname(dest), exist_ok=True)
    tmp = dest + ".tmp"
    open(tmp, "wb").write(data); os.replace(tmp, dest)
    ok += 1; rows.append((p["title"], f, "OK"))
open(mp, "wb").write(raw)
mark()
with open(os.path.join(target, "status.md"), "w") as s:
    s.write(f"# Course Pack status\n\nVersion: {ver} · tag: {tag or '-'} ({m.get('date','')})\n"
            f"Manifest: {MANIFEST}\nChecked: {time.strftime('%Y-%m-%d %H:%M %Z')}\n"
            f"Files OK: {ok} · Partial: {partial}\n\n| Page | File | Status |\n| --- | --- | --- |\n")
    for r in rows: s.write(f"| {r[0]} | {r[1]} | {r[2]} |\n")
print(json.dumps({"version": ver, "tag": tag, "pages": len(m["pages"]), "files_ok": ok, "partial": partial}))
```

## Version check (daily tidy-up on Mondays, and at session start when the last check is over 7 days old)

Run `python3 /workspace/spanish-teacher/course_setup.py --check-version`. Besides the Monday tidy-up, spanish-session-brief and spanish-lesson run it before a session whenever `course/last-version-check.txt` is missing or more than 7 days old, so a learner with no routines still gets pack updates. It takes a few seconds; if it fails, carry on with the cached files. It downloads only `main/manifest.json` and compares its `version` and `tag` with `course/manifest.json`; it downloads no files. `update_needed: false`: nothing to do. `true`: finish any lesson in progress (or the lesson about to start), then run this skill normally; files whose sha256 changed are downloaded from the new tag's `raw_url`, unchanged ones are kept.

## Pages

| Page | Repo path (local: `course/` + path; source: that page's `raw_url` in the manifest) |
| --- | --- |
| Index and manifest (current version) | `pages/00-index-manifest.md` |
| Learning plan overview | `pages/01-learning-plan-overview.md` |
| Course rules (13.1–13.11) | `containers/rules.md` |
| 13.12 Topic materials | `containers/topics.md` |
| 13.13 Exam papers | `containers/exams.md` |
| 13.1 Syllabus | `pages/rules/13.1-syllabus.md` |
| 13.2 Topic format | `pages/rules/13.2-topic-template.md` |
| 13.3 Worked example: A1 | `pages/rules/13.3-worked-example-a1.md` |
| 13.4 Worked example: A2 | `pages/rules/13.4-worked-example-a2.md` |
| 13.5 Worked example: B1 | `pages/rules/13.5-worked-example-b1.md` |
| 13.6 Worked example: B2 | `pages/rules/13.6-worked-example-b2.md` |
| 13.7 Worked example: C1 | `pages/rules/13.7-worked-example-c1.md` |
| 13.8 Teaching notes | `pages/rules/13.8-teaching-notes.md` |
| 13.9 Rubrics | `pages/rules/13.9-rubrics.md` |
| 13.10 Spaced review schedule | `pages/rules/13.10-spaced-review-schedule.md` |
| 13.11 Progress tracking | `pages/rules/13.11-progress-tracking.md` |
| Administration and scoring guide | `pages/exams/00-guide.md` |
| Placement test | `pages/exams/Placement.md` |
| A1 exam papers | `pages/exams/A1-exam.md` |
| A2 exam papers | `pages/exams/A2-exam.md` |
| B1 exam papers | `pages/exams/B1-exam.md` |
| B2 exam papers | `pages/exams/B2-exam.md` |
| C1 exam papers | `pages/exams/C1-exam.md` |
| C1 graduation oral | `pages/exams/C1-graduation-oral.md` |
| A1.1-1 Primeros contactos. Saludos y datos personales | `pages/topics/A1.1-1.md` |
| A1.1-2 La familia y los amigos. Descripción física y estados de ánimo | `pages/topics/A1.1-2.md` |
| A1.1-3 La alimentación | `pages/topics/A1.1-3.md` |
| A1.2-4 La ciudad y el transporte | `pages/topics/A1.2-4.md` |
| A1.2-5 Un día normal. Horarios, hábitos y costumbres | `pages/topics/A1.2-5.md` |
| A1.2-6 El tiempo libre. Organizar actividades de ocio | `pages/topics/A1.2-6.md` |
| A2.1-1 Nuestra experiencia. Hablar de nuestro pasado personal y profesional | `pages/topics/A2.1-1.md` |
| A2.1-2 Ir de compras. Necesidades y gustos | `pages/topics/A2.1-2.md` |
| A2.1-3 Nuestras biografías. Relatar y describir el pasado | `pages/topics/A2.1-3.md` |
| A2.2-4 Viajes. Turismo y planes de vacaciones | `pages/topics/A2.2-4.md` |
| A2.2-5 Las opiniones. Expresar el punto de vista | `pages/topics/A2.2-5.md` |
| A2.2-6 Los sentimientos. Expresar emociones y valoraciones | `pages/topics/A2.2-6.md` |
| B1.1-1 Encuentros. Presentarnos y conversar sobre nosotros mismos | `pages/topics/B1.1-1.md` |
| B1.1-2 Cambios. Hablar del pasado y su relación con el presente | `pages/topics/B1.1-2.md` |
| B1.1-3 Historias. Narrar el pasado e imaginar el futuro | `pages/topics/B1.1-3.md` |
| B1.2-4 La salud. Comida y vida sana. Enfermedades y remedios | `pages/topics/B1.2-4.md` |
| B1.2-5 La ciudad y la vivienda. Compras. Trámites administrativos | `pages/topics/B1.2-5.md` |
| B1.2-6 Los viajes y los servicios turísticos. Climas y lugares | `pages/topics/B1.2-6.md` |
| B1.3-7 Un paseo cultural: literatura, escultura y música | `pages/topics/B1.3-7.md` |
| B1.3-8 El poder de la imagen. Moda y publicidad | `pages/topics/B1.3-8.md` |
| B1.3-9 El mercado laboral. Empresas en España e Hispanoamérica | `pages/topics/B1.3-9.md` |
| B1.4-10 Relaciones y protocolo en actos sociales | `pages/topics/B1.4-10.md` |
| B1.4-11 Medio ambiente. Campo y ciudad | `pages/topics/B1.4-11.md` |
| B1.4-12 Aprender idiomas. La conciencia intercultural | `pages/topics/B1.4-12.md` |
| B2.1-1 ¿Nos conocemos? Carácter y personalidad. Formación y experiencia laboral | `pages/topics/B2.1-1.md` |
| B2.1-2 ¡Qué tiempos aquellos! Historias, sucesos, relatos y acontecimientos | `pages/topics/B2.1-2.md` |
| B2.1-3 Mirar hacia delante. Planes, intereses y deseos. Hechos futuros posibles e imposibles | `pages/topics/B2.1-3.md` |
| B2.2-4 Busco un lugar. Descripción de pueblos y ciudades | `pages/topics/B2.2-4.md` |
| B2.2-5 Me interesa tu opinión. Participación en la sociedad | `pages/topics/B2.2-5.md` |
| B2.2-6 Estoy intrigado. Establecer hipótesis | `pages/topics/B2.2-6.md` |
| B2.3-7 ¿Qué me recomiendas? Sugerencias, peticiones y consejos | `pages/topics/B2.3-7.md` |
| B2.3-8 Dicen que dicen… Contar qué dicen, sugieren o aconsejan otras personas | `pages/topics/B2.3-8.md` |
| B2.3-9 ¡Qué arte tiene! Manifestaciones artísticas del mundo hispano | `pages/topics/B2.3-9.md` |
| B2.4-10 Si lo hubiera sabido… Condiciones hipotéticas y reclamaciones formales | `pages/topics/B2.4-10.md` |
| B2.4-11 Alimenta que da gusto. Describir y valorar la gastronomía hispana | `pages/topics/B2.4-11.md` |
| B2.4-12 ¡Hagan juego! Juegos hispanos. Concursos, ofertas de trabajo, condiciones y reglas en Internet | `pages/topics/B2.4-12.md` |
| C1.1-1 Protagonistas: nosotros. Medios de comunicación. Biografías y autobiografías | `pages/topics/C1.1-1.md` |
| C1.1-2 Para eso están los amigos. Enfermedades y hábitos de salud. Ritos funerarios | `pages/topics/C1.1-2.md` |
| C1.1-3 Pero… ¿qué se habrán creído? Reclamaciones relacionadas con la vivienda | `pages/topics/C1.1-3.md` |
| C1.2-4 Se hace camino al andar. La emigración y los intelectuales hispanos en el exilio | `pages/topics/C1.2-4.md` |
| C1.2-5 Aquí hay trabajo. Comunicarse con éxito en el mundo laboral | `pages/topics/C1.2-5.md` |
| C1.2-6 Cada maestrillo con su librillo. La evolución de la enseñanza. Cambios personales | `pages/topics/C1.2-6.md` |
| C1.3-7 Busque, compare y si encuentra algo mejor… Consumo y publicidad | `pages/topics/C1.3-7.md` |
| C1.3-8 Viajar y viajar. Rutas históricas, turísticas y culturales hispanas | `pages/topics/C1.3-8.md` |
| C1.3-9 La historia me absolverá. Panorama político y literario de Iberoamérica | `pages/topics/C1.3-9.md` |
| C1.4-10 Un futuro verde. Ecología, recursos naturales, fuentes de energía y contaminación | `pages/topics/C1.4-10.md` |
| C1.4-11 El español y su literatura. Instituciones de la lengua y el fomento de la lectura | `pages/topics/C1.4-11.md` |
| C1.4-12 Dele que DELE. El humor. El DELE C1 | `pages/topics/C1.4-12.md` |

## Validation

- `status.md` has a line for every manifest page; files_ok + partial = 72 (the number of entries with a file).
- A page is OK only if its local file's sha256 equals the manifest's.
- The progress file, if it existed, is byte-for-byte unchanged by this skill.

## Failure

- Manifest unreachable after the retry: keep any existing cache and use it. With no cache, tell the learner in one line ("I can't reach the course files right now; I'll try again shortly. Meanwhile we can do a short practice chat.") and offer free conversation, recorded as Free talk. Retry at the next session start.
- A page Partial: that topic, check or exam is temporarily unavailable. Say so plainly, keep the learner's progress, and continue with what is available. Never write content from memory to fill a gap.

## Return

One line to the learner only when something is unavailable. To yourself: version and tag, files OK / Partial, and whether `progress.md` was created.

## Approvals

None needed: reads public files and writes only inside `/workspace/spanish-teacher/`.
