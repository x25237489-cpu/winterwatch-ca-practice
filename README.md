# Winter Watch — easy practice build

Winter Watch is an internal decision-support page for the fictional County Cavan roads team. It follows the [easy Winter Watch brief](https://bdaib-hub.pages.dev/pack/winter-watch-k7p2qm#easy). The Duty Engineer makes and records the actual decision.

## Architecture

The whole page is in **one self-contained `index.html`**: HTML, inline CSS, and inline JavaScript. GitHub Pages serves that file from the repository root. No build step, framework, backend, relay, or deployment secret is needed.

The page uses the brief's **bring-your-own-key** flow. Each user chooses OpenAI, Anthropic, Google Gemini, or Groq, enters their own provider key in Settings, and presses Save. The key is stored only in that browser's `localStorage` and sent directly to the chosen provider when that user asks a question. **Forget key** removes the saved connection. A public link does not transfer the key to anyone else. The lecturer's brief explicitly says testers will paste their own key.

No provider key is in this repository. The previously added `GROQ_API_KEY` GitHub Actions secret is unused by this static page; repository secrets cannot serve a live browser request.

## What the page does

- Fetches tonight's depots, routes, Open-Meteo forecast, and winter plan for every question.
- Applies the plan's ground-temperature, wet-road, priority, salt, truck, driver, and timing rules in JavaScript. The route board is calculated locally before the AI explanation.
- Reads all five winters of `history.csv` and reduces them to one summary row per route. It separates ice reports on treated and untreated nights, counts no-salt nights, and excludes an impossible-temperature row from analysis.
- Sends the AI the plan, short depot and route summaries, the ten historical summary lines, and the user's question. The full history file and depot notes are not sent as instructions.
- Shows provider errors, token usage, and an optional cost estimate from prices entered by the user.

The source `history.csv` has **7,560 rows, 439 gritting runs and 1,152.45 tonnes of salt**. The analytical table excludes one impossible-temperature row, leaving **7,559 valid route-nights, 438 runs and 1,149.15 tonnes**. The page displays both totals to make the data-quality decision visible.

## Use and verify

Open the [published page](https://x25237489-cpu.github.io/winterwatch-ca-practice/). The live data and rule-based route board work without an AI key. To get a live AI answer, add your own key in Settings. Saving it once makes it available after reload in the same browser; click **Forget key** on a shared device.

Before hand-in, check the public URL in a fresh browser: no key should be prefilled, the source checks should pass, and a live AI answer should appear only after that browser's user saves a valid key. Search the repository and commit history for any real key prefix. If a key was ever published, revoke it at the provider and create a new one.

Fill in [HANDIN.md](HANDIN.md) with the screenshot, five benchmark results, measured cost per question, and governance sentences after testing. The easy pack is practice; the actual CA2 hand-in instructions are on Moodle.

