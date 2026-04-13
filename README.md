# PriceLabs Deviation Analyzer

A lightweight, client-side web application that analyzes pricing deviations from a PriceLabs bulk listing export. No server, no database, no data ever leaves the browser.

---

## What It Does

Upload a PriceLabs bulk export file and the app will:

- Identify all dates where **Your Price** deviates significantly from **ADR Last Year**
- Visualize occurrence counts by weekday × month in an interactive heatmap
- Show competitor pricing, availability, and minimum stay alongside your own data
- Filter, sort, and export results

---

## How to Get the File from PriceLabs

> **Dynamic Pricing → Customization → Select PMS in the left panel → Download CSV from the ⋯ options on the right side**

Include up to 5 competitor columns (prices, availability & min stay) to this downloaded file.

---

## Supported File Formats

| Format | Notes |
|---|---|
| `.csv` | PriceLabs default export — leading `#Note:` comment line is stripped automatically |
| `.xlsx` | Excel format with the same column structure |
| `.xls` | Legacy Excel format |

Files with up to **10,000 rows** are handled efficiently. All processing is in-browser.

---

## Column Requirements

The app auto-detects columns by name (case-insensitive, partial match):

| Column | Required | Notes |
|---|---|---|
| `Date` | ✅ | YYYY-MM-DD or Excel date format |
| `Listing Name` | ✅ | Property identifier |
| `Your Price` | ✅ | Current listed price |
| `ADR Last Year` | ✅ | Prior year average daily rate |
| `ADR` | Optional | Current year ADR — rows where this is present are excluded (stay already realized) |
| `prices / available / min_stay` × 5 | Optional | Competitor data columns auto-detected from file header |

---

## Deviation Logic

```
Deviation % = (Your Price − ADR Last Year) / ADR Last Year × 100
```

**Row inclusion rules:**
- ✅ Include: `ADR Last Year` is present and non-zero
- ❌ Exclude: `ADR Last Year` is missing or zero
- ❌ Exclude: Current year `ADR` is already present (stay has been realized)

This ensures comparisons are only made for future/unbooked dates where last year's performance is the benchmark.

---

## Features

### Deviation Occurrence Heatmap
- **X-axis:** Months (Jan-YY → Dec-YY, year suffix auto-derived from data)
- **Y-axis:** Day of week (Mon–Sun, weekends highlighted in amber)
- Each cell split into **top half (green = positive count)** and **bottom half (red = negative count)**
- Color intensity scales with count relative to the maximum in the dataset
- **Click any half** to filter the table to exactly those rows — a filter strip appears showing what's active
- Heatmap updates live as you change the threshold or listing selector

### Filters

| Filter | Behaviour |
|---|---|
| Listing Name | Scopes heatmap + table to one listing |
| Day of Week | Restricts to a specific weekday |
| Min Deviation % | `40` = show \|dev\| ≥ 40%. `30` = show \|dev\| ≥ 30%. `-15` = show dev ≤ −15% |
| Direction | Both / Positive only / Negative only |

Default threshold is **40%** — shows both ≥ +40% and ≤ −40%.

### Deviation Results Table

Sortable columns: Date, Day of Week, Listing Name, Your Price, ADR Last Year, Deviation %

- Deviation % shown in **green** (positive) or **red** (negative)
- Weekends highlighted in amber
- **View** link on each row opens the Competitor Snapshot for that exact listing + date
- Paginated at 100 rows per page
- **Export CSV** downloads the current filtered view

### Competitor Snapshot Panel

Appears below the results table when a file with competitor columns is uploaded.

- Select any **listing** and **date** to see all competitors side by side
- Shows: Competitor name, Price, Availability (1 = available, 0 = not), Minimum Stay
- "Your Listing" self-reference entry included when present in the file
- Clicking **View** in the table auto-populates this panel for that row

---

## Competitors Detected (example from Quebec file)

| # | Competitor |
|---|---|
| 1 | SP102 – Les Lofts St-Pierre – By Les Lofts Vieux-QC (43678893) |
| 2 | SP105 – Lofts St-Pierre – By Les Lofts Vieux-QC (43688783) |
| 3 | Residence on "Place Royale" (51103965) |
| 4 | Unique house in the most scenic street of Qc. (54102782) |
| 5 | Your Listing – [self-reference in competitor format] |

Competitor names and column groupings are auto-detected from the file header — no manual configuration needed.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Vanilla HTML + CSS + JS — zero build step |
| Excel/CSV parsing | [SheetJS / xlsx](https://sheetjs.com) v0.18.5 via CDN |
| Fonts | Google Fonts — Syne, DM Sans, DM Mono |
| Deployment | Vercel (static site) |

No frameworks, no `npm install`, no build process required.

---

## Project Structure

```
pricelabs-analyzer/
├── index.html      ← entire application (UI + logic + styles in one file)
├── vercel.json     ← Vercel static deployment config
├── README.md       ← this file
└── .gitignore      ← excludes .vercel/, .DS_Store, node_modules/
```

---

## Local Development

No setup needed:

```bash
# Option 1: open directly
open index.html

# Option 2: any static server
npx serve .

# Option 3: Python
python3 -m http.server 3000
```

---

## Deployment 

### GIT and Vercel locations

1. Push this repo to GitHub: Samruddhi990PM -> PriceLabsDeviationAnalyzer
2. Vercel: samruddhi.waghchaure@strategycues.com -> StrategyCues Projects (Hobby) -> https://price-labs-deviation-analyzer

Your app will be live at https://price-labs-deviation-analyzer.vercel.app/

### Via Vercel CLI

```bash
npm i -g vercel
vercel --prod
```

---

## Pushing Updates

```bash
git add .
git commit -m "describe your change"
git push
```

Vercel auto-redeploys on every push to `main` within ~20 seconds.

---

## Privacy

All file processing happens entirely in the user's browser via the [File API](https://developer.mozilla.org/en-US/docs/Web/API/File_API). No data is uploaded to any server. Files are never stored, logged, or persisted in any form.

---

## License

Internal use. © Strategy Cues Revenue Intelligence Platform.
