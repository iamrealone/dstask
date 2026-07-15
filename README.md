# Trader Performance vs. Bitcoin Market Sentiment

Analysis of Hyperliquid historical trade data against the Bitcoin Fear & Greed Index, exploring how trader behavior and profitability shift across market sentiment regimes.
The datasets are huge to be uploaded here. So I skipped the upload of the datasets. But the process to follow for successfully executing this project is clearly stated below:
## Contents

| File | Description |
|---|---|
| `Trader_Sentiment_Analysis_Report.docx` | Full written report — findings, tables, charts, and strategic recommendations. Start here. |
| `Trader_Sentiment_Analysis.ipynb` | Reproducible Jupyter notebook — loads the raw CSVs, merges them, runs all stats/charts in the report. |
| `merged_trades_with_sentiment.csv` | Every trade record joined with that day's sentiment classification and index value. |
| `historical_data.csv` | Raw input — Hyperliquid trade log (not modified). |
| `fear_greed_index.csv` | Raw input — daily Bitcoin Fear & Greed Index (not modified). |

## How to reproduce

1. Place `historical_data.csv` and `fear_greed_index.csv` in the same folder as the notebook.
2. Open `Trader_Sentiment_Analysis.ipynb` in Jupyter (or VS Code / JupyterLab).
3. Run all cells top to bottom. Requires `pandas`, `numpy`, `scipy`, and `matplotlib`.
4. Outputs (`merged_trades_with_sentiment.csv`, summary CSVs, and chart PNGs) are written to the working directory.

## Methodology

- **Join:** Trades are matched to sentiment by calendar date (IST). 479 of 480 trading days had a matching sentiment reading.
- **Realized trades:** Analysis focuses on the 104,402 trades with non-zero `Closed PnL`, since PnL is only booked when a position is closed (see "What is PnL" below).
- **Sentiment grouping:** Used both at 5-level granularity (Extreme Fear → Extreme Greed) and collapsed into 3 buckets (Fear, Neutral, Greed) for cleaner comparison.
- **Risk-adjusted return:** Each trade's PnL is normalized by its position size (`Closed PnL / Size USD`) to compare regimes on a like-for-like basis, since raw dollar PnL is skewed by position size.

## What is PnL?

**PnL = Profit and Loss.** `Closed PnL` is the realized dollar gain or loss booked when a position is closed (as opposed to unrealized PnL on a position still open). Zero PnL rows are opening trades; non-zero rows are closes.

## Key findings

- Traders take **larger positions during Fear** (avg $7,375) than Greed (avg $4,234).
- Raw average PnL is similar across regimes, but **risk-adjusted returns strongly favor Fear** — return volatility during Greed is ~16x higher, driven by a handful of extreme trades (including liquidation-scale losses).
- **Shorts are far more profitable during Fear** (avg $186.52/trade) than longs in the same regime (avg $82.73) — the clearest directional edge in the data.
- The sentiment index alone is a **weak predictor of daily PnL** (correlation ≈ -0.10) but a useful signal for expected risk/variance.
- A small number of tokens (TRUMP, FARTCOIN) account for a disproportionate share of losses, regardless of sentiment.

Full detail, charts, and strategic implications are in the Word report.

## Limitations

- No explicit leverage/account-equity field in the data; position size (USD notional) was used as a proxy for risk appetite.
- Sentiment is a single daily, market-wide value — it doesn't capture intraday shifts or coin-specific sentiment.
- The trader sample (32 accounts) may not generalize to the broader Hyperliquid user base.
