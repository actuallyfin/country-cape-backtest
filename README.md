# Country CAPE Rotation Backtest

Interactive backtests for a country-level CAPE rotation strategy on a 44-country
MSCI IMI NETR universe, 1997–present. Entirely client-side — a single HTML page
loads a precomputed JSON grid of 2,016 strategy runs and renders summary stats,
a growth-of-$10k chart, and rolling 5-year CAGR.

## Live site

After enabling GitHub Pages on this repo (Settings → Pages → Deploy from `main`
branch, root), the site will be available at:

`https://<username>.github.io/country-cape-backtest/`

## What the user can vary

- **CAPE basket**: cheapest/most expensive 10 / 15 / 25 / 50 % by country CAPE,
  or no CAPE filter
- **Trend filter**: 150 / 200 / 250-day SMA uptrend or downtrend, or none
- **Momentum filter**: none, standard 12-month absolute momentum, or fresh-negative
  12/8 or 12/12
- **Country exclusion**: none, exclude best-Sharpe country, exclude worst,
  exclude both (robustness toggle)
- **Trading cost**: 0 or 10 bps/side
- **Benchmarks**: EW-44, MSCI ACWI IMI, MSCI Developed IMI, MSCI ACWI ex USA
  IMI, cash

9 × 7 × 4 × 4 × 2 = 2,016 combinations, all precomputed.

## Files

- `index.html` — the app (single file, loads Chart.js from CDN)
- `webapp_results.json` — precomputed grid of 2,016 runs + 5 benchmarks (~11 MB,
  ~2–3 MB gzipped)
- `.nojekyll` — disables GitHub Pages Jekyll processing

## Local preview

```
python -m http.server 8000
```

Then open `http://localhost:8000/`.

## Regenerating the data

The JSON is produced by `precompute_webapp_grid.py` in the parent research repo
(not published here). Regeneration takes ~13 minutes on a single thread and
requires the underlying valuation / price CSVs in `valuations_csv/`.

## Data notes

- **Stats** (CAGR / Vol / Sharpe / MaxDD) are computed on daily returns.
- **Monthly series** are stored only for chart rendering; stats are not derived
  from them.
- **Cash proxy**: BIL ETF total return post-2007-05-30; synthetic daily level
  built from FRED TB3MS pre-2007 and spliced to BIL at its first observation.
- **Non-reinvest mechanic**: filter-masked capital earns the cash rate instead
  of being redistributed.
- **Rebalance**: quarterly, 10 business days after the CAPE quarter-end to
  account for publication delay.
- **Country exclusion** uses full-sample look-ahead — it is a robustness lens,
  not a tradable signal.
