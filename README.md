# school-admission-watch

Tracks admission-info pages for a set of Tokyo/Saitama junior high schools so a
daily cloud routine can detect real content changes (not just re-notify every
day) and email the owner when something changes.

## How it works

- `schools.json` lists each school with the page(s) to watch.
- `state/<school-id>.json` holds the last-seen content hash + a short snapshot
  for each watched page, written by the routine after every run.
- On each run, the routine fetches the current page content, normalizes it,
  hashes it, and compares to the stored hash:
  - No `state/<school-id>.json` yet → first run, just save the baseline, no
    email.
  - Hash unchanged → nothing to do.
  - Hash changed → send an email describing what changed, then update the
    stored hash/snapshot and commit+push.
