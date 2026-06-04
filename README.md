# pcodes-assign

Browser-based spatial join tool that assigns P-codes and any other attributes from GeoJSON polygon boundaries to your point data (CSV or Excel) — entirely in the browser. No backend, no server, no data leaves your computer.

## How to use

1. **Upload data file**  
   Drop or select a `.csv`, `.xlsx`, or `.xls` file. The app shows the filename, total row count, and auto-detected column headers.

2. **Upload GeoJSON boundary file**  
   Drop or select a `.geojson` or `.json` file containing polygon features (any admin boundaries, villages, etc.). The app reads the feature count and lists every property key that will be appended to your rows.

3. **Select coordinate columns**  
   Choose the Latitude and Longitude columns from your data using the dropdowns. Common names are pre-selected when possible.

4. **Run spatial join**  
   Click **Assign**. Each row is turned into a point and tested with `turf.booleanPointInPolygon`. All GeoJSON feature properties from the containing polygon are added to that row. Points that fall outside every polygon receive empty strings for the boundary columns.

5. **Download result**  
   The output file is downloaded automatically with the same format as your input:
   - `.csv` → `_pcoded.csv`
   - `.xlsx` / `.xls` → `_pcoded.xlsx`  
   Filename = `original_filename_pcoded` + appropriate extension.

## Supported formats

- Data: `.csv`, `.xlsx`, `.xls`
- Boundaries: `.geojson`, `.json` (FeatureCollection, Feature array, or single Polygon/MultiPolygon feature)

## Privacy

All processing happens in your browser.  
No data is uploaded to any server.

## Performance

- Handles 10,000+ rows smoothly.
- Processing uses chunks of 500 rows with `setTimeout` yielding so the UI never freezes.
- Only numeric lat/lon values are used for the point-in-polygon test.

## Live demo

**Correct URL (after enabling Pages):** https://pyaesonekw.github.io/pcodes-assign

### How to enable GitHub Pages (fixes "This Connection Is Not Private" / certificate errors)

1. Go to your repo: https://github.com/pyaesonekw/pcodes-assign
2. Click **Settings** (top right) → **Pages** (in the left sidebar under "Code and automation").
3. Under "Build and deployment":
   - **Source**: select **Deploy from a branch**
   - **Branch**: `main`
   - **Folder**: `/ (root)`
4. Click **Save**.

GitHub will build and publish the site (usually 30–90 seconds). The first time it may take a bit longer. Once live, the URL above will show the app with a valid certificate.

**Why you saw the Safari warning:**
- You tried `https://pyaesone.pskw.github.io/...` (from your Gmail address).
- Your actual GitHub username is `pyaesonekw`.
- Even the correct hostname shows this error (or a 404) until Pages is explicitly enabled in the repo settings. GitHub only serves a valid TLS certificate for published Pages sites.

You can test everything right now by opening the `index.html` file directly in any browser (double-click it). No server needed.

## Tech

- Turf.js (point-in-polygon)
- SheetJS / xlsx (Excel read & write)
- PapaParse (CSV read & write)
- 100% vanilla JavaScript + inline CSS
- All libraries loaded from public CDNs (jsDelivr + official SheetJS CDN)
- Single self-contained `index.html` — works when opened from GitHub Pages or locally

## Development / local testing

Just open `index.html` in Chrome or Edge. No build step, no npm install.

To test the full flow you can use the built-in **"Load demo data"** button (adds 5 sample points + 2 sample polygons).

## License

MIT

---

Built for fast, private, no-install P-code / boundary assignment workflows.