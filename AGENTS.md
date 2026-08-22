# walletx-config

This repository is a **remote configuration / version-manifest repo** for the WalletX
Android app (`com.walletx.app`). Its entire tracked content is a single file:
`version.json`. There is no application source code, no package manager, no build
step, and no test/lint suite here.

`version.json` schema:

- `latest` — newest published app version.
- `minSupported` — minimum version still allowed to run (below this = force update).
- `android.playUrl` — Play Store URL used to send users to update.
- `message` — text shown in the update prompt.

The WalletX app fetches this JSON at runtime and compares the installed version
against `minSupported` (force update) and `latest` (optional update).

## Cursor Cloud specific instructions

- This is a **data-only repo**. There are no dependencies to install and nothing to
  build. The update script is intentionally a no-op.
- Lint/test/build: there is no configured lint, test, or build tooling. The only
  meaningful check is that `version.json` is valid JSON, e.g.
  `python3 -c "import json; json.load(open('version.json'))"` or `jq . version.json`.
- "Running the application": the config is served as a static file in production
  (raw GitHub / CDN). To exercise the core flow locally, serve the repo with
  `python3 -m http.server 8077` and fetch `http://localhost:8077/version.json`. A
  client then performs version-gating by comparing an installed version against
  `minSupported`/`latest`. The actual app code lives in a separate repository.
- When editing `version.json`, keep it valid JSON and preserve the schema keys above;
  the app depends on all of them being present.
