# Hidaya Analytics LP — Local Build

## Run
Open `index.html` in a modern browser. No server, backend, API connection, external libraries, or network connection is required.

## Current data included
- Android: monthly metrics and country summary previously compiled from Google Play Console exports.
- iOS: daily App Store Connect exports from 2022-05-24 through 2026-10-13:
  - First-Time Downloads: 7,521
  - Redownloads: 1,112
  - Total Downloads: 8,633
  - 54 monthly summaries and 99 territories with non-zero downloads.
- iOS territory total reconciles to the daily Total Downloads sum (8,633).
- Per-day First-Time Downloads + Redownloads reconciles to Total Downloads on every date in the supplied files.

## Platform and date behavior
- Platform selector: All, Android, iOS.
- iOS date filtering uses daily source rows, so exact date ranges are supported for app-level iOS download metrics.
- iOS territory filtering uses the supplied per-territory Total Downloads export. The supplied files do not provide First-Time Downloads and Redownloads split by territory; those component values are shown as N/A when a specific territory is selected rather than invented.
- Android remains based on monthly aggregated records; date ranges that overlap part of a month include the whole available month and are explicitly noted.
- `Total Downloads` means First-Time Downloads + Redownloads in these exports; it is not a unique-device count.
- The historical iOS snapshot is retained separately from the newly imported daily series. Do not add these values together.
- ISO territory codes are mapped from source territory names where available; original territory labels are preserved in iOS exports.

## Monthly manual update
On the first day of each month:
1. Update `data.js` using the newest approved Google Play Console monthly export and App Store Connect exports.
2. Append/correct Android monthly records in `androidMonthly` and country rows in `androidCountries`.
3. Append/correct iOS daily rows in `iosDaily`, monthly summaries in `iosMonthly`, territory totals in `iosTerritories`, and sparse territory-day rows in `iosTerritoryDaily`.
4. Update historical snapshots only from verified source exports and keep their `asOf` dates accurate.
5. Preserve source field names and don't fill missing dates with zero unless the source explicitly reports zero.
6. If a source definition/mapping is uncertain, keep it marked REVIEW in the Metrics Dictionary.
7. Test filters, language toggle, tables, chart, and CSV exports.
8. Do not publish to GitHub unless separately requested.

## Metric guardrails
- Android source metrics stay separate by field and platform.
- `Active Device Installs` is a daily stock/base metric; never sum daily values as unique devices.
- iOS `Total Downloads` includes first-time downloads and redownloads; it is not unique devices.
- Current iOS data includes a daily series, while the older snapshot remains separately identified.
- Missing reports/months must not be treated as zeros.

## Overview platform rules (implemented locally)
- Overview shows Android and iOS in separate sections by default; the platform filter can show either section alone.
- No shared Android+iOS total is calculated.
- Android figures are sourced from monthly records. If a selected date range overlaps only part of a month, the complete source-month value is shown and the row is labeled Partial with an explanatory tooltip.
- iOS figures are calculated from supplied daily records within the selected dates; territory-level First-Time Downloads and Redownloads remain N/A because that breakdown was not supplied.
- Each summary metric card includes a short definition. The Metrics Dictionary remains the detailed reference.
- The existing data.js was not modified during this Overview update.


## Overview layout (local update)
- Android and iOS are displayed in separate sections; there is no cross-platform grand total.
- Android's main Unique Users card uses the latest available Google Play snapshot and shows its as-of date.
- iOS's Unique Devices card uses First-Time Downloads as the agreed acquisition proxy, with a definition noting that the source reports downloads rather than a direct unique-device count.
- Each platform has its own metric selector and a concise definition on the selected metric card.
- The Overview shows the top 10 countries separately for Android installs and iOS Total Downloads; country codes remain present alongside English and Arabic names.
- Original `data.js` records are preserved; this UI change edits `index.html` only.
- Android remains monthly source data; selecting part of a month displays the full source-month value with a Partial note. iOS metrics use daily records inside the selected date range.
