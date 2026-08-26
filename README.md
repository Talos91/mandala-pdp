# Mandala PDP

Mobile-first tracker for Mandala (Mandalart / Harada-method 9×9) personal
development boards, for exactly two users. Single static HTML file, no build
step, no framework.

**App:** https://talos91.github.io/mandala-pdp/

## How it works

The app itself is public and contains **no board data**. All boards and the
tracking log live in a separate **private** GitHub repo as `state.json`,
read and written through the GitHub Contents API. On first open, each device
pastes an access token once (stored in localStorage, never in this repo).

- **Sync**: every change appends to an event log and pushes `state.json`
  (optimistic concurrency via blob sha; conflicts merge by event id and retry).
- **Offline**: changes land in localStorage and sync when back online.
- **Boards**: 9×9 view with per-pillar zoom; tap a cell to log hit/miss/skip
  for its period. Cadence is parsed from cell text (`1×/month`, `4x/week`,
  `every 2 months`, `biweekly`, `once a quarter`, `daily`, `by Dec 31`, …);
  cells without cadence are principles (keep/review).
- **Monthly review mode** walks every cell one screen at a time and writes a
  one-line summary to the log.
- **Edits** keep history; cells can be marked downgraded instead of deleted.

## Setup (once)

1. Create a **private** repo (default name `mandala-data`) containing a
   `state.json`:
   ```json
   { "rev": 1, "boards": [ { "id": "me", "name": "Me", "board": { "center": {"text": "…"}, "pillars": [ /* 8 × {key,name,half,cells[8]} */ ] } } ], "events": [] }
   ```
2. Create a **fine-grained personal access token** scoped to only that repo,
   permission Contents: Read and write.
3. Open the app, paste token + repo. Done. Second user: same token, same
   paste, their phone.
