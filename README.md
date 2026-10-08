# Academic job applications, 2026–27 cycle

## Layout

| Path | What it is |
|---|---|
| `tracker.csv` | One row per job. **The only place deadlines and status live.** |
| `master/` | Generic CV, statements and cover-letter template. Improve these over time. |
| `shared/jobapp.sty` | Shared look and your personal details (name, email...) for every document. |
| `applications/<institution-position-year>/` | Only for jobs where something is tailored. |
| `applications/_template/` | What `bin/new-application` copies. |
| `references/recommenders.md` | Letter writers and which jobs each was asked for. Local only (gitignored). |
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
(country, tenure-track or career teaching track, expired, not math) run in the
script; judgement calls (field, elite, pay) are recorded per ad in
`search/decisions.csv`. Ads with no
decision land in `review`: `bin/jobscan --show review`. It also lists tracked
jobs whose feed deadline moved or whose posting disappeared.

A daily Claude cloud routine ("Job listing monitor",
https://claude.ai/code/routines/trig_016eGRQi7SHHiBPTcCjiT8xj) runs
`bin/jobscan --daily` from the GitHub copy at 7am Eastern, adds its suggested
calls, and pushes to `main`. In `search/reports/`:

| File | What it is |
|---|---|
| `YYYY-MM-DD.md` | New ads that day, with suggested calls. **Every dated file still here needs your calls.** |
| `status.md` | Rewritten daily: last run (calls recorded, anything not understood), reports waiting, open maybes, tracked-job changes, deadlines in the next 30 days. |
| `history.md` | Every ad from finished reports with its suggestion, your call and the outcome, newest first. |

To decide: `git pull`, type `apply`, `maybe` or `skip` in a report's "Your call"
column (a reason helps the routine learn: `skip; because QIS record required`),
then commit and push (`git pull --rebase` first if the push is refused). The
next morning the routine records the calls (`decisions.csv`; apply also adds a
`tracker.csv` row and fills its letters, documents, portal and notes from the
ad), and once every ad in a report has a call it moves the report into
`history.md` and deletes it. An ad that disappears from the feeds before you
call it counts as done. Entries it can't read are listed in `status.md` and
stay pending until fixed.

Who writes what, so pushes don't conflict: you edit only the dated reports
(and `tracker.csv` as your applications progress); the routine creates dated
reports, never edits one after that day, and is the only writer of
`status.md`, `history.md` and the calls in `decisions.csv`. A call is final
once recorded; to change one, edit `decisions.csv` and `tracker.csv` or ask
Claude.

The routine also reads your past calls ("Learning from Joel's calls" in
`search/criteria.md`) so new suggestions follow your precedents. It fills any
tracker row whose letters and documents are both empty, so you can also add a
job by hand with just institution, position, deadline and link. Its cloud
environment ("Default") must allow mathjobs.org and academicjobsonline.org.

## Building

Open the folder in VS Code and choose **Reopen in Container**. LaTeX
Workshop builds a `.tex` file on save. Every document starts with
`\usepackage{jobapp}`, which the container finds in `shared/` from any folder.
Generated PDFs aren't committed, except those in `submitted/` folders.

## Git

Private GitHub repository (pushed so the cloud routine can read it). Commit
after each submission so the history records what went out when, and push
after changing `decisions.csv` or `tracker.csv` so the routine sees them.
Confidential letters (`references/letters/`) and the letter-writer list
(`references/recommenders.md`) are gitignored and never pushed.
