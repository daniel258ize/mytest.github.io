# Progress Knight Quest

A continuation of Progress Knight 2.0. This is a browser-based idle/incremental game featuring five prestige layers and various unique mechanics. The app is a static website (HTML/CSS/JS) with no build toolchain.

Bug-fix PRs are welcome.

## Overview
- Stack: Static site — HTML, CSS, vanilla JavaScript
- Vendor library: A prebuilt, minified math engine at `js/math.js`
- Entry point: `index.html`
- Runtime: Runs entirely in the browser; saves/settings are stored in `localStorage`
- Hosting: Can be opened directly from the file system or served by any static HTTP server (e.g., GitHub Pages)

## Requirements
- A modern Chromium-based browser (Edge/Chrome recommended)
- Optional: A simple static HTTP server for local development
  - Python (if available): `python -m http.server 8080`
  - Any other static server also works

Note: Some browsers impose stricter restrictions for `file://` URLs. If you see missing resources or security warnings when double‑clicking `index.html`, serve the folder instead.

## Getting Started (Run Locally)
1. Clone or download this repository.
2. Open `index.html` in a modern browser.
   - If you prefer serving over HTTP:
     - Python: `python -m http.server 8080` (then open http://localhost:8080/)
     - Any simple static server on Windows/PowerShell also works.

## Scripts
This project does not use a package manager or build tools (no npm/yarn/pnpm). There are no project scripts to run.

- TODO: If a build/test workflow is introduced in the future, document the commands here.

## Configuration / Environment Variables
No environment variables are required. The game reads and writes persistent data to the browser's `localStorage` under this origin.

- Tip: When testing flows that depend on a “fresh” state, clear `localStorage` for this origin.

## Testing
There is no formal test runner in this repo. A lightweight, browser‑based test harness works well for the existing structure:

- Create a simple page under `tests/` that loads just the modules you need (for example `js/utils.js`) and performs assertions.
- On success, set `document.title = "PASS"` and write `ALL TESTS PASSED` into the body so it can be detected in headless runs.

Example minimal flow (guideline):
1. Create `tests/utils.test.html` that loads `js/utils.js` and runs assertions for pure helpers like:
   - `formatTime(sec, show_ms)`
   - `yearsToDays(years)`, `daysToYears(days)`, `getCurrentDay(days)`
   - `getBaseLog(base, value)`
   - `bigIntToExponential(bigint)` (ensure you pass BigInt values, e.g., `123n`)
2. Open `tests/utils.test.html` in your browser and verify the page title shows `PASS` or the body contains `ALL TESTS PASSED`.
3. Optional headless run (adjust path as needed):
   - Chrome: `"C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe" --headless=new --disable-gpu --allow-file-access-from-files --virtual-time-budget=4000 --dump-dom file:///C:/path/to/repo/tests/utils.test.html > tests/_dom.txt`
   - Edge: `"C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe" --headless --disable-gpu --virtual-time-budget=4000 --dump-dom file:///C:/path/to/repo/tests/utils.test.html > tests/_dom.txt`

Guidelines for adding tests:
- Keep each test standalone under `tests/` and load only the modules it needs.
- Stub globals only when absolutely necessary and isolate side effects.
- Prefer testing pure logic (math/formatting/time). For UI logic, create minimal DOM fixtures as needed.
- Do not commit temporary test artifacts unless you intend to maintain them.

## Project Structure
High-level layout:

- `index.html` — Main entry point and UI templates consumed by JS
- `css/` — Stylesheets (`styles.css`, `dark.css`, `colorblind.css`, `currencies.css`, `w3.css`)
- `img/` — Icons and images
- `js/` — Game logic and utilities, including:
  - `main.js` — Game loop and orchestration
  - `ui.js` — UI rendering and tab management
  - `utils.js` — Utility functions (formatting, time helpers, etc.)
  - `data.js`, `classes.js`, `tooltips.js`, `challenges.js`, `dark_matter.js`, `evilperks.js`, `metaverse.js`, `milestones.js` — Feature modules
  - `HackTimer.js` — Timer shim used by the game
  - `math.js` — Large prebuilt/minified math library (vendor; not intended for manual edits)
- `changelog.txt` — Human-readable changelog
- `README.md` — This file

## Development Notes
- Global state: Some utilities reference globals (e.g., `gameData`, `numberNotation`). Keep dependencies explicit and guard against missing globals to keep helpers testable.
- Number formatting: `utils.format` depends on a global math object (from `js/math.js`) and `gameData.settings.numberNotation`. Provide sane fallbacks when globals are absent in tests.
- Currency formatting: `formatCoins()` reads `gameData.settings.currencyNotation` and writes into a supplied DOM element containing child spans for parts. For tests, construct a minimal DOM scaffold and stub `gameData`.
- Time formatting: `formatTime()` supports optional milliseconds and negative durations and is safe to unit test without the game runtime.
- Performance: The game updates frequently; prefer batching DOM writes and using `textContent` over `innerHTML` in hot paths.

## Deployment
- This repo can be hosted directly on GitHub Pages using the repo root as the site root. All assets are served from relative paths.
- Any static file host will work; no server-side components are required.

## Changelog
See `changelog.txt` for recent changes.

## License
No license file is present in this repository.

- TODO: Choose and add a `LICENSE` file (e.g., MIT, GPL-3.0, Apache-2.0) and reference it here.
