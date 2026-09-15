# T1 Project Plan

## Project Goal

Develop a clear and reproducible research workflow for comparing three familiar asset classes using illustrative ETFs:

- `SPY` — US equities
- `TLT` — long-term US Treasury bonds
- `GLD` — gold

## Available Data

`data/etf_snapshot.csv` contains one row per illustrative ETF with the following columns, documented in `data/data_dictionary.md`:

| Column | Meaning |
|---|---|
| `ticker` | ETF identifier |
| `asset_class` | Broad asset type |
| `expected_return_pct` | Illustrative annual return assumption |
| `volatility_pct` | Illustrative annual variability assumption |
| `max_drawdown_pct` | Illustrative largest peak-to-trough loss |
| `expense_ratio_pct` | Illustrative annual fund fee |

The dataset is synthetic teaching data: all values are illustrative assumptions, not live quotations, historical estimates, or forecasts.

## Expected Final Deliverable

A concise comparison of the three ETFs (SPY, TLT, GLD) across the available metrics, supported by a reproducible analysis script, so that any learner can rerun the workflow on the same fixed dataset and obtain the same result.

## Three Project Milestones

1. **Project setup (T1)** — Create this written plan and confirm the repository structure and dataset are in place.
2. **Analysis (planned)** — Write a script to compute and compare expected return, volatility, max drawdown, and expense ratio for each ETF.
3. **Review and summary (planned)** — Verify the results, summarize the trade-offs between equities, bonds, and gold, and document the workflow.

## One Data Limitation

The dataset is synthetic teaching data and intentionally omits correlations, taxes, transaction costs, liquidity, currency exposure, and investor-specific constraints; therefore it must not be used as investment advice or as the basis for a real investment decision.

## Next Action

Have the project owner review this plan; after approval, save it with Git (commit and push are performed by the user, not by JiuWenSwarm) and proceed to the analysis milestone.
