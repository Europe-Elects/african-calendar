# African Electoral Calendar

A rolling calendar of African elections: the past six months (collapsed) and the next six months, with a map,
filters by election type, and the result of past elections (column "Result" of the sheet).

## How it works

- A scheduled GitHub Action (`.github/workflows/update-data.yml`) reads the Google Sheet every 3 hours with the keyless
  Google login (robot account `ee-sheets-reader@project-6c08a079-4a01-46eb-aaf.iam.gserviceaccount.com`) and saves the
  merged data as `data.json`.
- `index.html` loads only `data.json`. The sheet's address is a repository secret and never appears in the site.
- To refresh straight away: **Actions → Update election data → Run workflow**.

## Set up

1. Share the calendar Sheet with the robot account above (Viewer, "Notify people" off).
2. Settings → Secrets and variables → Actions → New repository secret:
   - `SHEET_ID` = the long ID in the sheet's URL, between `/d/` and `/edit`
   - `SHEET_GID` = the number after `gid=` for the tab (here `11896539`)
3. Settings → Pages → Deploy from a branch → `main`, folder `/ (root)`.
4. Actions → Update election data → Run workflow. It creates `data.json`; the page is then at
   https://europe-elects.github.io/<repo>/

## Sheet requirements

- Headers in row 1: `Country`, `Specification`, `Type`, `Mode`, `Sequence`, `(Expected) Date`, `Date Status`, `Comment`, `Result`.
- Dates as `12 January 2025` (ISO `2025-01-12` and day-first `12/01/2025` also work).
- `Date Status` exactly `Confirmed` for confirmed dates; anything else shows as **tbc**.
- `Comment` = `Not Free/Fair` adds that tag.
- `Result` is shown on the event (for elections with rounds: the latest round that has a result).
