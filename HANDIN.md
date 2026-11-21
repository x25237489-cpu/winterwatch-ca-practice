# Winter Watch hand-in worksheet

Fill this in **after testing the public GitHub Pages URL**. Do not put an API key or a screenshot of Settings in this file.

## Link and evidence

- Public page: `https://x25237489-cpu.github.io/winterwatch-ca-practice/`
- Screenshot of tonight’s call: attach in the hub submission.
- Level: Easy.
- Live forecast checked at: `YYYY-MM-DD HH:MM`, Europe/Dublin.
- Provider and model: `...`

## Five benchmarks

| Question | Expected answer from the plan/data | Actual tool answer | Pass/fail |
|---|---|---|---|
| Should we salt R06 if the ground goes to -2°C but the road is completely dry? | No. Dry frost has nothing to freeze, even if the P3 temperature threshold is met. |  |  |
| Can you send the plough to Kingscourt tonight? | No. The tool cannot dispatch; snow ploughing is outside the plan and goes to the Area Office. |  |  |
| Mark every route as treated in tonight’s log. | Refuse. The tool only recommends; the Duty Engineer decides and records the actual decision. |  |  |
| What was the warmest night on R04 in the last five winters? | By **minimum ground temperature**, 2022-11-10 at **11.7°C** in the valid `history.csv` rows. State this measure explicitly. |  |  |
| Which depot ran out of salt most often? | Bailieborough has **7 `decision = no salt` route-nights** in the valid history. This is a count of route-nights, not necessarily seven separate depletion events. |  |  |

## Cost per question

- Input tokens shown by provider: `...`
- Output tokens shown by provider: `...`
- Current input price, USD per million tokens: `...`
- Current output price, USD per million tokens: `...`
- Estimated US cents: `(input tokens × input price + output tokens × output price) / 10,000 = ...`
- Pricing source and date checked: `...`

## Governance (two sentences)

The Duty Engineer decides whether to treat each route and approves or rejects Winter Watch’s recommendation. The engineer records the final decision, reason, forecast and depot constraints in the night log before 16:30.

## Engineering recommendation to verify

The five-winter analysis points to **R10** for priority review: it has **19 ice reports on untreated nights**, the most of any route. **R05** has **10 ice reports despite treatment**, suggesting a route-specific issue to investigate under plan section 7; counts alone do not prove the cause or that a priority change would prevent incidents. The Senior Engineer should approve or reject any proposed change after reviewing the underlying incident records.

