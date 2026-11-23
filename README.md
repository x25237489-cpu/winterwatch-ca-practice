# Winter Watch — GitHub Pages with a private Groq relay

Winter Watch is a decision-support page for the fictional County Cavan roads team. The [public page](https://x25237489-cpu.github.io/winterwatch-ca-practice/) remains a self-contained `index.html` on GitHub Pages. It fetches live data and calculates route decisions in the browser. A small Node.js service on Render calls Groq using a key stored in Render's Environment settings, so visitors do not enter a key and the key is never published in HTML or GitHub.

## Setup

The Render web service is [winterwatch-ca-api](https://dashboard.render.com/web/srv-dc20ii3l550s73fp6m6g), built from this repository's `main` branch. It starts with `npm start` and serves `https://winterwatch-ca-api.onrender.com/ask`. Its code is in `relay/server.mjs` and has no external packages.

1. Open the Render service's **Environment** tab. Add `GROQ_API_KEY` with your Groq key as its value, then select **Save and deploy**. Never put the value in this repository, `index.html`, `package.json`, or chat.
2. Visit `https://winterwatch-ca-api.onrender.com/health`. It should return `{"status":"ok","configured":true}` after the deploy. This confirms the service sees a key; it does not prove Groq will accept it.
3. Open the public GitHub Pages link in a fresh browser. No key field should appear. Ask the default question and verify a live answer and token counts.
4. Check Groq usage and set an organization spend limit if your account supports one. The public endpoint can consume your allowance. Groq's paid-tier spend limits are documented at https://console.groq.com/docs/spend-limits.

The free Render service may sleep after 15 minutes idle and take about a minute to wake. The page checks its health on load and retries for up to 150 seconds before attempting a question; it then shows a retry message if the service still cannot be reached. The relay has request-size, per-IP, global minute, and daily in-memory limits. Those counters reset when Render restarts, and CORS Origin checks are not authentication against non-browser callers. A publicly usable bot cannot keep the ability to spend your key completely private from abuse; monitor usage and revoke the key if needed.

The `GROQ_API_KEY` GitHub Actions repository secret previously added is **not** used by this service. GitHub Pages cannot read an Actions secret at request time. The key must be set in Render separately.

## Easy-brief alignment

The page still uses one self-contained HTML file and calculates the ground-temperature, wet-road, priority, salt, truck, driver, and timing rules in JavaScript. It reads 7,560 historical rows, checks 439 runs and 1,152.45 tonnes of source salt, and excludes one impossible-temperature row from the analysis. The model receives compact summaries, not the full historical file.

**Important:** the written easy practice brief says testers will paste their own key and that the page should call a provider directly from the browser. This shared-key deployment follows the user's newer requirement that anyone with the link can use the chatbot without a key. It is an intentional change from that practice brief. Confirm the actual CA2 marking instructions on Moodle before submitting this as the graded version.

Fill in [HANDIN.md](HANDIN.md) with a real screenshot, five benchmark results, measured cost per question, and governance sentences after testing.

