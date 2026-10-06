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
  container (`jdvillatoro/latex-devenv`); Claude can't compile, so ask Joel to
  build and paste errors. `bin/deadlines` runs in WSL directly.
- Local git only: no GitHub remote. Commit with
  `git -c user.name="Joel Villatoro" -c user.email=41701387+JDVillatoro@users.noreply.github.com`.
