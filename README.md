# GCU Lamina

Open any file — even a multi-gigabyte one — and **scroll, filter, sort, derive,
summarize, and quality-check** it. Windowed (never loads the whole file),
read-only, **networkless** — your data never leaves the page.

Live at **https://gentropic.org/lamina** · on the **Microsoft Store** as
*GCU Lamina* · or [download `lamina.html`](https://gentropic.org/lamina/lamina.html)
— one self-contained file to keep, share, or run offline.

## What it does

- **Delimited → grid** (CSV/TSV, GSLIB/Geo-EAS, whitespace dumps), **text → lines**,
  **binary → hex**. Opens **Datamine `.dm`** tables directly — decoded on the fly,
  no conversion, any size.
- Reads inside `.zip` / `.tar` / `.gz` / `.zst` / `.bz2`, and windows huge
  compressed entries without unpacking.
- **Filter** with SQL-`WHERE` syntax (`grade > 1 and lito = "OXIDE"`), sort,
  find, jump; **calculated columns** from formulas; per-column **statistics**
  (histogram + quantiles), one-pass **Σ stats** over every column, **group-by**
  with weighted means, and a **data-quality** scan for the silent bugs
  (leading-zero codes, `-999` sentinels, junk in numeric columns).
- **Grade–tonnage** — tonnes · tonnage-weighted grade · contained metal by domain,
  plus per-grade **cutoff curves**, from expression-defined volume × density ×
  ore-proportion. Windowed: a multi-GB block model reports without loading.
- **Export** the current view (filtered · sorted · derived) to CSV/TSV, streamed
  to a file — never uploaded. Save your view as a small `.lamina` **lens** and
  re-apply it to the next export.

## Security posture

Sealed: CSP `connect-src 'none'` — the browser itself enforces that nothing is
sent anywhere. No accounts, no server, no telemetry, WASM-free. Works from
`file://` and air-gapped machines. Verify the build and read the IT note at
**https://gentropic.org/security**.

## What this repo is

The **deploy surface** only — the PWA shell (`manifest.webmanifest`, `sw.js`,
icons) lives here. The app itself is built in the
[auditable](https://github.com/gentropic/auditable) monorepo
(`ext/lamina` + `tools/lamina`); its single-file build (`node build.js
--target=lamina`) lands here as `lamina.html`.

`publish.mjs` wraps `lamina.html` into `index.html` (PWA injection); the GitHub
Pages workflow (`.github/workflows/publish.yml`) deploys it. The workflow prefers
the latest auditable release's `lamina.html`, falling back to the committed seed,
so it deploys whether or not a release has cut yet.

## Updating

- **From a release:** auditable's release uploads `lamina.html` and triggers this
  repo's `publish.yml`.
- **Dev refresh:** in `../auditable`, `node build.js --target=lamina`, copy
  `lamina.html` here, commit, push — Pages redeploys.
- **Service worker:** `node build-sw.mjs` regenerates `sw.js` from `@gcu/sw`
  (needs `../auditable` checked out); the result is committed.
- **Microsoft Store:** the Store app is a hosted PWA — content deploys reach it
  automatically; only a *manifest* change (icons, file handlers, display) needs a
  PWABuilder repackage + resubmission.

MIT © Arthur Endlein Correia — Geoscientific Chaos Union, gentropic.org
