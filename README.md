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
(country, tenure-track, expired, not math) run in the script; judgement calls
(field, elite, pay) are recorded per ad in `search/decisions.csv`. Ads with no
decision land in `review`: `bin/jobscan --show review`. It also lists tracked
jobs whose feed deadline moved or whose posting disappeared.

A daily Claude cloud routine ("Job listing monitor",
https://claude.ai/code/routines/trig_016eGRQi7SHHiBPTcCjiT8xj) runs the same
scan from the GitHub copy at 7am Eastern. It writes `search/reports/latest.md`
(ads needing a call with suggested calls, tracked-job changes, deadlines in the
next 30 days, bucket counts), plus a dated copy `search/reports/YYYY-MM-DD.md`
when something is new, and pushes them to `main`. Besides
`search/reports/` it only fills in new `tracker.csv` rows (below). `git pull`
to read the reports; pull before pushing your own changes.

To decide on new ads: `git pull`, type `apply`, `maybe` or `skip` in the
report's "Your call" column (optionally with a reason: `skip; because QIS
record required`), save, and run `bin/calls`. It records the calls in
`decisions.csv` (apply also adds a `tracker.csv` row), commits those two files,
discards your edits to the report, then pulls with rebase and pushes. Don't
commit `search/reports/` yourself: the routine owns it, so your commits never
conflict with its daily one. Recorded ads drop out of the next report.

The routine responds to your calls the next morning: it fills the `letters`,
`documents`, `portal` and `notes` of any tracker row where letters and
documents are both empty (so you can also add a row by hand with just
institution, position, deadline and link), and it reads your past calls
(`decisions.csv`, "Learning from Joel's calls" in `search/criteria.md`) so
that new suggestions follow your precedents. Reasons you give with a call
help it most.

The routine's cloud environment ("Default") must allow mathjobs.org and
academicjobsonline.org.

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
