# PriceLabs Deviation Analyzer

A lightweight, client-side web app that analyzes pricing deviations from a PriceLabs Excel export — no server, no database, no data stored.

## Features

- 📤 **Drag-and-drop upload** of `.xlsx` / `.xls` / `.csv` files
- 📊 **Auto-detects columns**: Date, Listing Name, Your Price, ADR Last Year
- 📅 **Day-of-week extraction** with weekend highlighting
- 🎯 **Filters deviations > 40%** (configurable threshold)
- 🟢🔴 **Color-coded deviation %** (green = positive, red = negative)
- 🔃 **Sortable columns**
- 🔍 **Filter by**: Listing Name, Day of Week, Direction, Min Deviation %
- ⬇️ **Export filtered results to CSV**
- ⚡ **Handles up to 10,000 rows** with pagination
- 🔒 **100% client-side** — no data leaves the browser

## Stack

- Vanilla HTML/CSS/JS — zero build step
- [`xlsx`](https://github.com/SheetJS/sheetjs) for Excel parsing (CDN)
- [Google Fonts](https://fonts.google.com): Syne + DM Sans + DM Mono

## Excel File Format

The app expects an Excel file with these columns (names are matched flexibly):

| Column | Description |
|---|---|
| `Date` | Date of the pricing |
| `Listing Name` | Property/listing identifier |
| `Your Price` | Current listed price |
| `ADR Last Year` | Average Daily Rate from the previous year |

## Deviation Formula

```
Deviation % = ((Your Price - ADR Last Year) / ADR Last Year) × 100
```

Rows where `|Deviation %| > 40%` are flagged.

## Deploy to Vercel

### Option 1: One-click (Vercel Dashboard)
1. Push this repo to GitHub
2. Go to [vercel.com](https://vercel.com) → **Add New Project**
3. Import your GitHub repo
4. Click **Deploy** — no build settings needed

### Option 2: Vercel CLI
```bash
npm i -g vercel
vercel --prod
```

## Local Development

No build step required — just open the file:

```bash
# Any static server works
npx serve .
# or
python3 -m http.server 3000
# or simply open index.html in a browser
```

## Privacy

All processing happens entirely in the user's browser via the [File API](https://developer.mozilla.org/en-US/docs/Web/API/File_API) and [SheetJS](https://sheetjs.com). No data is uploaded to any server.
