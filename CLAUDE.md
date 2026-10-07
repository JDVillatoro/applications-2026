# Job applications 2026 (academic)

Joel's applications for academic math jobs, 2026–27 cycle. See README.md for
layout, tracker columns and workflow.

## Conventions

- `tracker.csv` is the single source of truth for deadlines and status. Don't
  duplicate them in `info.md` files. Dates are ISO `YYYY-MM-DD`; status values
  are listed in README.md. Keep it valid CSV (quote fields containing commas).
- Application folders: `applications/<institution>-<position>-<year>`, lowercase
  with hyphens. They're created only when something is tailored; make them with
  `bin/new-application`.
- Personal details (name, address, email) are defined once in
  `shared/jobapp.sty`; documents use `\myname`, `\myemail`, etc.
- `submitted/` PDFs are frozen records of what was sent: never edit or rebuild
  them.
- Not part of the notes toolkit (pure LaTeX, no pandoc). Builds need the dev
  container (`jdvillatoro/latex-devenv`). A Claude session started from WSL
  can't compile, so ask Joel to build and paste errors. The devcontainer installs
  the Claude Code VS Code extension, and a session started there runs inside the
  container and can run `latexmk` itself. `bin/deadlines` runs in WSL directly.
- Private GitHub remote (`origin`), read by the daily job-monitor cloud routine.
  Commit with
  `git -c user.name="Joel Villatoro" -c user.email=41701387+JDVillatoro@users.noreply.github.com`
  and push after changing `search/decisions.csv` or `tracker.csv`. Never commit
  anything under `references/letters/` or `references/recommenders.md`
  (letter writers stay local; both are gitignored).
- Job screening: `bin/jobscan` + `search/decisions.csv` (see README "Finding
  jobs"). Every ad gets a bucket and a written reason; missing data never
  excludes an ad.
