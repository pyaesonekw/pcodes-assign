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

**The only URL that works with GitHub's default certificate right now:**

https://pyaesonekw.github.io/pcodes-assign

### Step-by-step: Enable GitHub Pages (do this first)

1. Go to the repository: https://github.com/pyaesonekw/pcodes-assign
2. Click the **Settings** tab (near the top).
3. In the left sidebar, click **Pages**.
4. Under "Build and deployment":
   - **Source**: Select **Deploy from a branch**
   - **Branch**: Select `main`
   - **Folder**: Select `/ (root)`
5. Click **Save**.

GitHub will start publishing the site. This usually takes 30–90 seconds (sometimes longer on first publish). Refresh the URL above until you see the app instead of the 404.

Once published, GitHub automatically uses its `*.github.io` wildcard certificate, which is valid for `pyaesonekw.github.io`.

### Why `https://pyaesone.pskw.github.io/pcodes-assign` gives an SSL error

Your diagnosis is **correct**.

- GitHub serves a single wildcard certificate for `*.github.io` (and a few related names) issued by Let's Encrypt.
- This certificate only matches **one level** of subdomain: `yourusername.github.io`.
- It does **not** match multi-level names like `pyaesone.pskw.github.io`.
- Additionally, GitHub usernames cannot contain dots, so `pyaesone.pskw` is not a valid GitHub username.
- When you visit the wrong hostname, GitHub's servers present the `*.github.io` certificate, Safari detects the name mismatch, and shows the scary "This Connection Is Not Private" warning.

There is no way to make `pyaesone.pskw.github.io` work as a GitHub Pages site using GitHub's default infrastructure and certificates.

### If you want a custom domain (e.g. something with "pyaesone.pskw")

You can use a real domain you own. GitHub will automatically provision a free Let's Encrypt certificate for it.

**Recommended approach with Cloudflare (or any DNS provider):**

1. Register a domain you control (examples: `pcodes.pskw.dev`, `pyaesone.pskw.app`, `pcodes-assign.com` — very cheap).
2. In your DNS provider (Cloudflare is excellent and free):
   - Create a **CNAME** record:
     - Name/Host: `pcodes` (or `app`, `pcodes-assign`, whatever you want)
     - Target/Value: `pyaesonekw.github.io`
     - **Important (Cloudflare)**: Set Proxy status to **DNS only** (grey cloud) at least initially. Proxied (orange) can break GitHub's domain verification.
3. Go back to your repo → **Settings → Pages**.
4. Under "Custom domain", enter your full subdomain (e.g. `pcodes.pskw.dev`) and click **Save**.
5. Wait for GitHub to verify the domain (check the DNS records it tells you to add if needed — usually just the CNAME).
6. Once verified, check the box **Enforce HTTPS**. GitHub will obtain and install the certificate for your custom domain.

After this, your site will be available at the nice custom URL with a valid certificate, and you can update the README, share links, etc.

You can keep both the `pyaesonekw.github.io` URL and the custom domain active.

### Quick testing (no GitHub Pages required)

Double-click `index.html` from the folder on your computer. The entire app (uploads, spatial join, Excel/CSV output, progress, everything) runs locally in the browser with no server.

### What won't work

- Renaming the repository (`pcodes-assign`) only changes the path part of the URL, not the subdomain problem.
- You cannot "claim" `pyaesone.pskw.github.io` through GitHub Pages.

I have already updated the README in the repository with the correct URL and these instructions.

Let me know which path you want to take (standard GitHub URL vs. custom domain) and I can help update links, add a `CNAME` file if needed, or make any other changes.

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