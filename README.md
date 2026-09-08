# Forecast Accuracy RCA — demo

A self-contained, single-file analytics application for demand-planning forecast
accuracy. Open the HTML in a browser and it runs: no build, no server, no
dependencies beyond one vendored spreadsheet parser.

**Every figure in it is synthetic.** The dataset is generated deterministically
in the browser from a seeded RNG, over an invented product taxonomy and a set of
Asian markets. Nothing here comes from any employer's systems.

## What it does

Six views over a forecast-accuracy fact table at reference x target-month x horizon grain:

| View | Question it answers |
|---|---|
| Overview | Where is accuracy now, and how has it moved against last year? |
| Deep dive analysis | Which drivers explain the gap, ranked, at eight configurable grains |
| BU Breakdown | Forecast, actual, absolute error and accuracy per business unit and product line |
| Top Deviation | Largest over- and under-forecasts, with a drill-down to reference level |
| FA Tracker | Accuracy trend by business unit and by country, current year against prior |
| Touchless | Automation rate, baseline against enrichment, and touch versus target |

## The parts worth reading

- **Ingest** (`normalizeExport`, `validateFA`): unpivots a wide pivoted planning
  export into long rows, merges several uploads from disparate scopes, de-duplicates
  currency measures, and rejects corrupt rows via a non-physical value cap.
- **Grain discipline**: actuals are identical across forecast horizons, so every view
  filters to a single horizon. Aggregating without that double counts, which is the
  most common way this kind of dashboard lies.
- **Driver ranking with a volume floor**: a low-volume line can post a spectacular
  accuracy percentage on nothing. The floor auto-scales to a fraction of scoped
  actual and is overridable in Advanced, so tiny-volume fake winners stay out of
  the rankings.
- **Everything is local**: uploaded files are parsed in memory in the browser.
  Nothing is sent to a server or an external API.

## Running it

Open `forecast-accuracy-rca.html` directly, or serve the folder:

```bash
python3 -m http.server 4322
```

Drop your own planning export onto the upload panel to replace the demo data, or
place a `fa_data.csv` beside the file and it will load on start.

## Provenance

This is a rebuilt, fully sanitized version of a tool I designed and shipped at work
for demand planners. The employer's branding, product taxonomy, product-line master
and performance figures have all been removed and replaced with invented equivalents.
What remains is the design and the engineering.
