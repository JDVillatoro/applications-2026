# Job search criteria

Filters for screening ads. Add new rules here as they come up.

## Include

- Tenure-track positions at assistant professor level (including open-rank ads
  that allow assistant).
- USA and Canada.
- R1 universities through liberal arts colleges, including teaching-focused
  institutions (tenure-track teaching specialist roles count).

## Exclude

1. **Unrelated research area**: the ad restricts the field to something outside
   the research profile (see `research-profile.md`, "Not a fit").
2. **Hyper-elite institutions**: Harvard, MIT, Princeton, Stanford, Caltech,
   Berkeley, Chicago, Columbia, Yale, NYU (Courant). Other highly competitive
   places (e.g. Michigan, JHU, Rice, UT Austin, Williams) are kept but marked
   as reaches. *Draft list: Joel to confirm.*
3. **Low pay**: stated starting salary below $80k, i.e. a range whose bottom
   is under $80k is excluded even if its top reaches it (confirmed by Joel
   2026-10-06). Ads that don't state pay are kept; note when the institution
   type makes low pay likely.
4. **Faith requirements**: the ad requires a statement of faith or religious
   affiliation (e.g. Cedarville). Being asked to engage with a religious
   institution's mission (Villanova, Notre Dame) is *not* a reason to exclude.
5. Senior-only, chair and director positions.

## Sources

- MathJobs public JSON feed:
  `https://www.mathjobs.org/jobs/public_job_boards?limit=1000&page=1`
  (fields include country, type, deadline, subject, salary, full description).
  A deadline of `0000-00-00` means "open until filled", not expired; keep
  those ads. Check `open_date_raw` and the start date to drop leftovers from
  the previous cycle.
