# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

LeadGen is a single-file, zero-dependency lead generation web app (`index.html`). Open it directly in any browser — no build step, no server, no npm.

## Architecture

Everything lives in `index.html` in three inline sections:

1. **`<style>`** — Dark-theme CSS using CSS custom properties (`--bg`, `--panel`, `--accent`, etc.). Layout uses CSS Grid; the results table uses `overflow-x: auto` for mobile.

2. **`<body>`** — Static HTML: header, configuration card (API key + form fields), status bar `#status-bar`, results card `#results-section` (hidden until data loads).

3. **`<script>`** — Vanilla JS, no framework. Key globals:
   - `ACTOR_ID` — the Apify actor to run (currently `compass~crawler-google-places`)
   - `FIELD_MAP` — maps logical column names (`name`, `email`, `phone`, …) to arrays of actor-specific field aliases, resolved case-insensitively
   - `currentLeads[]` — holds the last fetched result array

### Apify API flow

```
generateLeads()
  → startRun(token, query)        POST /v2/acts/{ACTOR_ID}/runs
  → pollStatus(token, runId)      GET  /v2/actor-runs/{runId}  (every 3 s, 5-min timeout)
  → fetchResults(token, datasetId) GET /v2/datasets/{datasetId}/items
  → renderTable(leads)
```

### Dynamic table rendering

`renderTable()` builds both `<thead>` and `<tbody>` at runtime:
- Fixed columns (`Business Name`, `Category`, `Email`, `Phone`, `Website`) are resolved via `FIELD_MAP` using `resolve(lead, aliases)` — a case-insensitive alias lookup.
- Any extra fields returned by the actor that aren't covered by `FIELD_MAP` are appended as additional columns automatically via `extraKeys()`.
- URLs and emails in cells get smart rendering (mailto/external links).

## Switching actors

To point the app at a different Apify actor:
1. Change `ACTOR_ID` at the top of the `<script>`.
2. Update the `body` object inside `startRun()` to match the new actor's input schema.
3. Update `FIELD_MAP` aliases to match the new actor's output field names.
4. Adjust the `fixedCols` array in `renderTable()` (and the copy in `exportCsv()`) if the column labels should change.

## Apify plan notes

- The current actor (`compass~crawler-google-places`) works on the **free Apify plan** via API.
- Actors that show `"Users on the free Apify plan can run the actor through the UI and not via other methods"` in the ERROR field require a paid plan — switch actors rather than trying to work around this.
- The token is persisted to `localStorage` under the key `leadgen_apify_token`.
