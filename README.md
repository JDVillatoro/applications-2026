# Academic job applications, 2026–27 cycle

## Layout

| Path | What it is |
|---|---|
| `tracker.csv` | One row per job. **The only place deadlines and status live.** |
| `master/` | Generic CV, statements and cover-letter template. Improve these over time. |
| `shared/jobapp.sty` | Shared look and your personal details (name, email...) for every document. |
| `applications/<institution-position-year>/` | Only for jobs where something is tailored. |
| `applications/_template/` | What `bin/new-application` copies. |
| `references/recommenders.md` | Letter writers and which jobs each was asked for. |
| `search/` | Job search: `criteria.md`, `research-profile.md`, screening `decisions.csv`, `deep-dive.md` notes, latest `scan.csv`. |
| `bin/` | Small helper scripts (below). |

## Tracker columns

`tracker.csv` opens as a spreadsheet in VS Code. Dates are `YYYY-MM-DD` so
they sort correctly.

| Column | Contents |
|---|---|
| `institution`, `position` | e.g. `Washington University`, `Asst Prof` |
| `deadline` | `2026-11-15` (the date for full consideration) |
| `status` | `interested` → `preparing` → `submitted` → `interview` → `offer`; or `rejected` / `withdrawn` / `closed` |
| `portal` | `MathJobs`, `AJO`, `Interfolio`, `university` |
| `link` | URL of the posting or portal page |
| `letters` | number of letters required |
| `documents` | short codes: `CL CV RS TS DS PL` (cover letter, CV, research/teaching/diversity statement, publication list), plus anything unusual |
| `folder` | name under `applications/`, empty if only master documents are used |
| `notes` | anything short |

## Workflow

1. Found a job: add a row to `tracker.csv` (`status = interested`).
2. Tailoring something: `bin/new-application washu-asst-prof-2026`. This creates
   the folder with `info.md` (paste the full posting there) and a copy of the
   cover letter. Copy a master statement in too only if you tailor it.
3. Submitted: copy the exact PDFs you uploaded into the job's `submitted/`
   folder, set `status = submitted`, and commit.
4. See what's coming up: `bin/deadlines` (open jobs, sorted by deadline, with
   days left), or `bin/deadlines --all`.

## Finding jobs

`bin/jobscan` downloads the MathJobs and AcademicJobsOnline feeds and puts
every ad in one bucket with a reason (`search/scan.csv`). Mechanical filters
(country, tenure-track, expired, not math) run in the script; judgement calls
(field, elite, pay) are recorded per ad in `search/decisions.csv`. Ads with no
decision land in `review`: `bin/jobscan --show review`. It also lists tracked
jobs whose feed deadline moved or whose posting disappeared.

A daily Claude cloud routine ("Job listing monitor",
https://claude.ai/code/routines/trig_016eGRQi7SHHiBPTcCjiT8xj) runs the same
scan from the GitHub copy at 7am Eastern and sends a push notification listing
new `review` ads with a suggested call, plus tracked-job changes. It only reads
the repo. In a local session, "review new ads" reruns the scan, records the
calls in `decisions.csv` (and `tracker.csv` for Apply), and pushes, so the next
run doesn't report the same ads. Its cloud environment ("Default") must allow
mathjobs.org and academicjobsonline.org.

## Building

Open the folder in VS Code and choose **Reopen in Container**. LaTeX
Workshop builds a `.tex` file on save. Every document starts with
`\usepackage{jobapp}`, which the container finds in `shared/` from any folder.
Generated PDFs aren't committed, except those in `submitted/` folders.

## Git

Private GitHub repository (pushed so the cloud routine can read it). Commit
after each submission so the history records what went out when, and push
after changing `decisions.csv` or `tracker.csv` so the routine sees them.
Confidential letters (`references/letters/`) are gitignored and never pushed.
