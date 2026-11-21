# Winter Watch — easy practice build

Winter Watch is a static decision-support page for the fictional County Cavan roads team. It follows the [easy Winter Watch brief](https://bdaib-hub.pages.dev/pack/winter-watch-k7p2qm#easy). The page calculates route recommendations from live source data; a chosen AI provider explains them. The Duty Engineer makes and records the actual decision.

## What the easy brief requires

1. A public `winterwatch` repository and GitHub Pages URL, with an `index.html` that calls **your own** AI provider directly. The key is entered in Settings and saved in this browser; it is never in source code.
2. On every question, fetch tonight’s depots, routes, Open-Meteo forecast and plan. Use the **ground** forecast, not air temperature. Apply the priority thresholds, wet test, salt stock, truck and driver checks, and route timing rules in code.
3. Read all five winters of `history.csv` and reduce them to one row per route. Show rows, runs, salt, ice reports on treated nights, ice reports on untreated nights and no-salt nights. Exclude impossible temperature rows from analysis.
4. Answer the Senior Engineer’s route priority question using those numbers, one evidence limit and an approval decision.
5. Submit the public page link, a screenshot, five benchmark results, cost per question in cents and two governance sentences through the hub. The practice brief says the CA2 itself is on Moodle.

## Files

- `index.html`: the one-page interface and source links.
- `style.css`: layout, tables and mobile styling.
- `app.js`: source loading, CSV parsing, checks, route decisions, AI calls and token-cost display.
- `HANDIN.md`: a hand-in worksheet with expected benchmark answers.

No build step, package manager or framework is required. Open `index.html` in a browser with internet access, or publish these three site files to GitHub Pages.

## Source checks and data quality

The source `history.csv` has **7,560 rows, 439 gritting runs and 1,152.45 tonnes of salt** (1,152.5 tonnes rounded to one decimal). One row contains an impossible temperature above 40°C. The source totals include it because they check that the entire file was read. The analytical table excludes it: **7,559 valid route-nights, 438 runs and 1,149.15 tonnes** (1,149.2 rounded). This distinction is deliberate and is shown in the page.

The live forecast and depot sheet can change; a screenshot is evidence only for the time it was taken. Departure times are estimates from the first forecast freezing hour and route length. Any route labelled **Review timing** needs a human timing decision.

## Provider and key

Choose OpenAI, Anthropic, Google Gemini or Groq, then enter a current model ID and your own key in Settings. The provider endpoints and required headers follow the brief. No key is present in this repository. The page uses `localStorage` because the brief specifies browser-only persistence; use **Forget key** on a shared computer. Enter current model input and output prices from your provider to calculate cost per question; the page does not assume a price.

Once saved, the key is used automatically for every question in that browser, including after a reload, until **Forget key** is pressed. A Groq key saved by the repository's earlier Part 1 page is carried over automatically. The password field is blank after saving because the page already has the saved connection; it is only for replacing the key. A public link does not transfer browser storage to another person's browser. The brief says the marker tests the page with **their own** key, so the student's key must never be put in the link or source code.

The page sends the AI the plan, a short operational depot table, ten calculated route lines, and ten historical summary lines. Depot notes are never sent to the AI, since the live sheet contains a note addressed to an assistant. The full 7,560-row file is not sent to the provider.

## Publish on GitHub Pages

Use the existing public [`winterwatch-ca-practice`](https://github.com/x25237489-cpu/winterwatch-ca-practice) repository. Upload `index.html`, `app.js`, `style.css`, `README.md`, and `HANDIN.md` at the repository root. Under **Settings → Pages**, publish from the default branch and `/ (root)`. Open the resulting `https://x25237489-cpu.github.io/winterwatch-ca-practice/` page and run a test question with your own key. Search the repository for the first six characters of your key and confirm no results. If a key is ever published, revoke it at the provider and create a new one.

## What to verify before hand-in

- History source checks show **PASS** and the valid analysis totals are distinguished from source totals.
- A live board refresh shows all ten routes, source forecast values, treatment reasons, and depot constraints.
- Your own provider answers from the public page and displays token usage.
- The five benchmarks in `HANDIN.md` have actual answers and pass/fail marks.
- The screenshot shows the public URL and the live call, without the key field visible.

The later **hard** build will require independently sourced map points, forecast API and weather archive, plus an operations-to-weather join. Those sources are intentionally supplied in the easy build.

