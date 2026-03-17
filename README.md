# GRISI — Global Retail Investor Sentiment Index

> A daily-updated contrarian indicator that tells you: **what's the historical win rate if you enter the market today?**

[![US Dashboard](https://img.shields.io/badge/Dashboard-US_(EN)-blue)](https://wenyuchiou.github.io/grisi/index.html)
[![TW Dashboard](https://img.shields.io/badge/Dashboard-台灣_(中文)-orange)](https://wenyuchiou.github.io/grisi/tw.html)

## What It Does

Open the dashboard → see three things:

1. **Current score** — US and TW retail sentiment (0–100, high = greedy)
2. **Expected return** — based on 16 years of data, when sentiment was at this level, what happened next
3. **Signal** — HOLD / BUY / SELL with reasoning

**Example:** When the score is ~30 (fearful), SPY has historically returned **+2.2% over the next 20 days with 73% win rate**. At ~70 (greedy), only +0.8%.

## How It Works

```
Public market data (Yahoo Finance + FinMind)
  → 5 indicators per market, Z-score normalized (252-day rolling)
  → Behavioral adjustment at extremes (loss aversion, FOMO, herding)
  → Score 0–100
  → Historical forward return lookup at current score level
  → Daily auto-update via GitHub Actions
```

### What goes into the score

**US (SPY):** VIX complacency · SPY vs 52W high · 20d momentum · VIX-SPY correlation · Gold/SPY ratio

**TW (TAIEX):** US 10Y rate pressure · Gold/SPY ratio · realized vol · TAIEX vs 52W high · volume surge

### Why it works

Retail investors systematically overreact at extremes. When they're greedy (score >70), markets tend to underperform. When they're fearful (score <30), markets outperform. This is a well-documented behavioral finance phenomenon (Kahneman & Tversky, 1979).

### Backtest (2010–2026)

| Market | Horizon | IC | p-value | Interpretation |
|--------|---------|-----|---------|----------------|
| SPY | 20d | **-0.175** | < 0.0001 | High greed → low future returns |
| SPY | 60d | **-0.180** | < 0.0001 | Stronger over longer horizons |
| TAIEX | 20d | **-0.128** | < 0.0001 | Same pattern in Taiwan |

## Data Sources

| Data | Source | Update |
|------|--------|--------|
| SPY, VIX, Gold, 10Y yield | Yahoo Finance (yfinance) | Daily |
| TAIEX, TSMC | Yahoo Finance (yfinance) | Daily |
| Margin balance (融資餘額) | FinMind API (TWSE OpenData) | Daily |
| Institutional flows (三大法人) | FinMind API (TWSE OpenData) | Daily |
| Behavioral parameters | Prospect Theory literature | Static |

## Auto-Update

GitHub Actions runs Mon–Fri at 22:00 UTC (6PM ET / 6AM TWN):
1. Pulls latest market data
2. Updates dashboard JSON
3. Commits and pushes → GitHub Pages auto-deploys

Agent narrative (cultural sentiment analysis) is updated manually by Claude.

## Project Structure

```
grisi/
├── docs/           ← Dashboard (GitHub Pages serves from here)
│   ├── index.html  ← US market (English)
│   └── tw.html     ← TW market (繁體中文)
├── data/           ← All data + results
├── src/            ← Python scoring engine + pipelines
└── .github/        ← Daily auto-update workflow
```

## Not Financial Advice

GRISI is a research tool based on historical patterns. Past performance does not guarantee future results. Use at your own risk.
