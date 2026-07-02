# 360 Spin QC Review — GitHub Pages edition

Fully static site (hosted on GitHub Pages). Reads SKUs from your sheet and saves verdicts
back to a `Reviews` tab through a Google Apps Script bound to that sheet — no server of your own.

## Files
- `index.html` — the app (this is what GitHub Pages serves).
- `apps-script.gs` — paste into the sheet's Apps Script; it is the read/write backend.

## Setup

### 1. Backend (one time)
1. Open your spreadsheet → **Extensions → Apps Script**.
2. Delete the default code, paste all of `apps-script.gs`, **Save**.
3. **Deploy → New deployment → Web app.** Execute as **Me**, Who has access **Anyone**. Deploy.
4. Authorize when prompted, then **copy the Web app URL** (ends with `/exec`).

The script auto-finds the tab that has `sku_id`, `image_logic_iframe_url`, `flare_iframe_url`,
and creates the `Reviews` tab automatically.

### 2. Host the frontend on GitHub Pages
1. Push `index.html` to a repo (e.g. `nandmodi/360-qc-review`).
2. Repo → **Settings → Pages** → Source: **Deploy from a branch** → `main` / root → Save.
3. Open the published URL (`https://nandmodi.github.io/360-qc-review/`).

### 3. Connect
On first open it asks for the Apps Script `/exec` URL. Paste it, add your name, **Connect**.
It's saved in your browser; change it anytime via **Settings**.

## Using it
- `sku_id` centered on top; Spin 1 (image_logic) and Spin 2 (flare) side by side.
- **Better** row: Checkbox 1 = spin 1, Checkbox 2 = spin 2 (mutually exclusive; the winning card glows).
- **Save & Next** writes the verdict to the sheet and jumps to the next SKU.
- Keys: `1`/`2` pick · `0` clear · `←`/`→` browse · `Enter` save & next.
- **☰ SKUs** opens a searchable/filterable list; **Export CSV** dumps all verdicts.

## The one trade-off vs the Vercel version
There's no spin proxy (that needed a server). The spins embed directly in the iframe.
If `assets.spyne.ai/360` refuses to be framed, the panel will be blank — use the **open ↗**
link, or tell me the frame-image API for a `sku_id` and I'll build a native drag-to-rotate
viewer that doesn't depend on embedding at all.

## Note on verdict updates
If updates get slow on a very large sheet, the `doPost` reads column A each call to upsert
by `sku_id`. It's fine for thousands of rows; flag it if you hit tens of thousands.
