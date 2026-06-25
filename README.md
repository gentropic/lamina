# lamina

Open any file — even a multi-gigabyte one — and **scroll & filter** it. Windowed,
read-only, offline. Delimited → grid, text → lines, binary → hex handoff; peeks
inside `.zip` / `.gz`, and windows huge compressed entries without unpacking.

Live at **https://gentropic.org/lamina**.

## What this repo is

The **deploy surface** only — the PWA shell (`manifest.webmanifest`, `sw.js`,
`icon.svg`) lives here. The app itself is built in the
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
