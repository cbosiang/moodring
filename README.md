# MoodRing

Contrarian sentiment indicator for 5 equity markets (US, TW, JP, KR, EU).

Scores retail investor sentiment daily on a 0–100 scale using market-derived signals, then looks up historical forward returns under similar sentiment conditions.

**[Dashboard (EN)](https://wenyuchiou.github.io/moodring/)** · **[儀表板 (中文)](https://wenyuchiou.github.io/moodring/tw.html)**

## How It Works

1. Pull daily data: prices, VIX, RSI, margin balance, institutional flows
2. Normalize each component as a rolling 252-day Z-score (no lookahead)
3. Combine into a 0–100 sentiment score per market
4. Look up quintile-conditional forward returns from the 16-year backtest (2010–2026)

Score interpretation:
- **< 30**: Retail is fearful — historically, forward returns are above average
- **30–70**: Neutral range
- **> 70**: Retail is greedy — historically, forward returns are below average

## Markets & Data Sources

| Market | Index | Sentiment Inputs | Source |
|--------|-------|-----------------|--------|
| US | SPY | VIX, RSI, put/call proxy, 52w high distance | Yahoo Finance |
| Taiwan | TAIEX | Margin balance, foreign investor flows, TSMC margin | Yahoo Finance + FinMind |
| Japan | Nikkei | RSI, 52w high distance, return momentum | Yahoo Finance |
| Korea | KOSPI | RSI, 52w high distance, return momentum | Yahoo Finance |
| Europe | STOXX50 | RSI, 52w high distance, return momentum | Yahoo Finance |

## Auto-Update

GitHub Actions runs `daily_update.py` Mon–Fri at 14:00 UTC (22:00 Taipei time). The pipeline fetches data, recalculates scores, generates narratives, and pushes updated JSON to GitHub Pages.

## Backtest Summary

| Market | IC (20d) | p-value | Extreme Fear 20d Avg Return |
|--------|----------|---------|---------------------------|
| US | -0.175 | < 0.0001 | +4.17% |
| Taiwan | -0.161 | < 0.0001 | +6.45% |
| Japan | -0.148 | < 0.0001 | +5.81% |
| Korea | -0.152 | < 0.0001 | +9.16% |
| Europe | -0.143 | < 0.0001 | +4.28% |

Negative IC means higher sentiment scores predict lower forward returns (contrarian signal).

## Project Structure

```
src/
  daily_update.py      # Daily data pipeline
  backtest.py          # US/TW backtest engine
  backtest_expansion.py # JP/KR/EU expansion
data/                  # JSON data files
docs/                  # GitHub Pages dashboard
  index.html           # EN dashboard
  tw.html              # TW dashboard
  data/                # Served JSON for frontend
```

## Author

[Wenyu Chiou](https://linkedin.com/in/wenyu-chiou) · Lehigh University

*Not financial advice. Past performance does not guarantee future results.*
