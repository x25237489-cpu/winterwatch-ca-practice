# Winter Watch — easy practice build

Winter Watch is a static decision-support page for the fictional County Cavan roads team. It follows the [easy Winter Watch brief](https://bdaib-hub.pages.dev/pack/winter-watch-k7p2qm#easy). The page calculates route recommendations from live source data; a chosen AI provider explains them. The Duty Engineer makes and records the actual decision.

## What the easy brief requires

1. A public `winterwatch-ca-practice` repository and GitHub Pages URL. The written easy pack describes direct browser calls with a key entered in Settings. For a marker to use the link without entering a key, this project also includes a student-owned Cloudflare Worker relay. The key is never in source code or the published page.
2. On every question, fetch tonight’s depots, routes, Open-Meteo forecast and plan. Use the **ground** forecast, not air temperature. Apply the priority thresholds, wet test, salt stock, truck and driver checks, and route timing rules in code.
3. Read all five winters of `history.csv` and reduce them to one row per route. Show rows, runs, salt, ice reports on treated nights, ice reports on untreated nights and no-salt nights. Exclude impossible temperature rows from analysis.
4. Answer the Senior Engineer’s route priority question using those numbers, one evidence limit and an approval decision.
5. Submit the public page link, a screenshot, five benchmark results, cost per question in cents and two governance sentences through the hub. The practice brief says the CA2 itself is on Moodle.

## Files

- `index.html`: the one-page interface and source links.
- `style.css`: layout, tables and mobile styling.
- `app.js`: source loading, CSV parsing, checks, route decisions, AI calls and token-cost display.
- `config.js`: the public Worker URL after deployment; contains no key.
- `worker/`: the private Groq relay and Cloudflare configuration.
- `.github/workflows/deploy-relay.yml`: deploys the Worker from GitHub Actions, copying the already-saved `GROQ_API_KEY` secret into Cloudflare's private secret store.
- `HANDIN.md`: a hand-in worksheet with expected benchmark answers.

The GitHub Pages frontend requires no build step, package manager or framework. The Worker is deployed separately by GitHub Actions.

## Source checks and data quality

The source `history.csv` has **7,560 rows, 439 gritting runs and 1,152.45 tonnes of salt** (1,152.5 tonnes rounded to one decimal). One row contains an impossible temperature above 40°C. The source totals include it because they check that the entire file was read. The analytical table excludes it: **7,559 valid route-nights, 438 runs and 1,149.15 tonnes** (1,149.2 rounded). This distinction is deliberate and is shown in the page.

The live forecast and depot sheet can change; a screenshot is evidence only for the time it was taken. Departure times are estimates from the first forecast freezing hour and route length. Any route labelled **Review timing** needs a human timing decision.

## Private relay for a shared link

The retired [class Luas relay](https://luas-class.kluxy.app/stops) explicitly says to build your own free Cloudflare Worker. This project follows that pattern for Groq. The public page sends one compact question to the Worker; the Worker reads `GROQ_API_KEY` from its secret environment, calls Groq, and returns only the answer and token usage. The key never appears in page JavaScript, URLs or responses.

1. Keep `GROQ_API_KEY` in **GitHub repository secrets → Actions**. It is already saved there.
2. In a Cloudflare account, find the **Account ID** and create an API token scoped to **Edit Cloudflare Workers**. Save them as GitHub Actions repository secrets named `CLOUDFLARE_ACCOUNT_ID` and `CLOUDFLARE_API_TOKEN`. Do not paste any secret into a file or chat.
3. Open the repository's **Actions → Deploy Winter Watch relay → Run workflow**. The workflow deploys `worker/index.js` and uploads `GROQ_API_KEY` as a Worker secret.
4. Copy the deployed `https://…workers.dev` URL from the workflow result. Verify its `/health` response has `configured: true`. Put its `/ask` URL into `config.js` as `window.WINTERWATCH_RELAY_URL = 'https://…workers.dev/ask';` and commit that one public URL.
5. Reload the GitHub Pages link in a clean browser. Settings should say **Private AI connection configured**, no key field should appear, and a question should receive an answer with token usage.

The Worker limits calls to ten per minute per IP and fifty per minute overall, and caps each answer at 4,096 completion tokens. These limits are approximate; set a small spend limit in your Groq account before sharing the public link. The Worker only accepts browser calls from the GitHub Pages origin, but Origin is not authentication against non-browser clients.

Until `config.js` contains the deployed Worker URL, the page retains its direct browser-key mode so the current practice page still works.

## Direct browser-key fallback

In fallback mode, choose OpenAI, Anthropic, Google Gemini or Groq, then enter a current model ID and your own key in Settings. The provider endpoints and required headers follow the written pack. No key is present in this repository. The page uses `localStorage` for browser-only persistence; use **Forget key** on a shared computer. Enter current model input and output prices from your provider to calculate cost per question; the page does not assume a price.

Once saved, the key is used automatically for every question in that browser, including after a reload, until **Forget key** is pressed. A connection saved by the first `winterwatch` repository or this repository's earlier Part 1 page is carried over automatically because both GitHub Pages project sites share the same origin. **Forget key** removes these saved copies. The password field is blank after saving because the page already has the saved connection; it is only for replacing the key. A public link does not transfer browser storage to another person's browser.

The page sends the AI the plan, a short operational depot table, ten calculated route lines, and ten historical summary lines. Depot notes are never sent to the AI, since the live sheet contains a note addressed to an assistant. The full 7,560-row file is not sent to the provider.

## Publish on GitHub Pages

Use the existing public [`winterwatch-ca-practice`](https://github.com/x25237489-cpu/winterwatch-ca-practice) repository. GitHub Pages publishes from the default branch and `/ (root)`. The page is at `https://x25237489-cpu.github.io/winterwatch-ca-practice/`. Once the relay URL is configured, test this link in a clean browser with no saved key. Search the repository for the first six characters of your key and confirm no results. If a key is ever published, revoke it at the provider and create a new one.

## What to verify before hand-in

- History source checks show **PASS** and the valid analysis totals are distinguished from source totals.
- A live board refresh shows all ten routes, source forecast values, treatment reasons, and depot constraints.
- The private Worker answers from the public page with no key prompt and displays token usage.
- The five benchmarks in `HANDIN.md` have actual answers and pass/fail marks.
- The screenshot shows the public URL and the live call, without the key field visible.

The later **hard** build will require independently sourced map points, forecast API and weather archive, plus an operations-to-weather join. Those sources are intentionally supplied in the easy build.

