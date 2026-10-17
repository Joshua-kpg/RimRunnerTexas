# RimRunnerTexas

Unofficial Texas HS boys basketball ratings. Not affiliated with the UIL, TAPPS, the SPC or MaxPreps.
Live site: https://rimrunnertexas.pages.dev/ (backup copy: GitHub Pages).

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project `rimrunnertexas`.

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 05:30 and 12:00 UTC
(11:30 pm / 6:00 am Central standard time) and on manual dispatch. Before the Texas boys season opens
(2026-11-05, TAPPS first games; UIL boys games start Nov 13; `SEASON_START` in index.html) the script makes no requests.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Texas scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
