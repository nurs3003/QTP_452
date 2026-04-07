# QTP_452 — Sector Rotation Strategy

**FIN 452: Financial Analytics and Trading**
Nursultan Amangeldiev — University of Alberta, Winter 2026

---

## Overview

A systematic sector rotation strategy applied to the 11 S&P 500 Select Sector SPDR ETFs. The strategy combines three signals:

1. **Monthly K-Means clustering** — groups sectors into Growth, Neutral, and Defensive based on six trailing performance features (return, volatility, Sharpe, skewness, kurtosis, 6-month momentum)
2. **Daily Kalman Filter signal** — tracks each sector's excess return vs SPY in real time; fires only when a sector is persistently diverging from the market, not just moving with it
3. **Death cross regime filter** — activates the short leg only when the short-term SMA crosses below the long-term SMA, preventing shorts from dragging in bull markets

**Universe:** XLB, XLC, XLE, XLF, XLI, XLK, XLP, XLRE, XLV, XLU, XLY + SPY benchmark
**Training:** June 2000 – December 2024
**Testing (OOS):** January 2025 – March 2026

---

## Results

| | Strategy | SPY |
|---|---|---|
| IS Ann. Return | +5.7% | +7.9% |
| IS Sharpe | 0.29 | 0.41 |
| OOS Ann. Return | **+15.0%** | +7.6% |
| OOS Sharpe | **0.90** | 0.41 |
| OOS Max Drawdown | −16.7% | −18.8% |

---

## Files

| File | Description |
|------|-------------|
| `QTP_452_v4.qmd` | Main Quarto document — all analysis, code, and write-up |
| `QTP_452_v4.html` | Self-contained rendered output (submit this) |
| `CLAUDE.md` | Project instructions for Claude Code |

---

## How to Render

Requires R with the following packages: `tidyverse`, `tidyquant`, `tidymodels`, `tidyclust`, `KFAS`, `PerformanceAnalytics`, `moments`, `cluster`, `factoextra`, `plotly`, `scales`, `kableExtra`, `slider`

```r
quarto::quarto_render("QTP_452_v4.qmd", output_format = "html")
```

Or from the terminal:

```bash
quarto render QTP_452_v4.qmd --to html
```

Data is pulled live from Yahoo Finance at render time via `tidyquant`. No local data files are needed or included.

---

## Key Parameters

All tunable constants are at the top of the `{r setup}` chunk:

| Constant | Default | Description |
|----------|---------|-------------|
| `K_CLUSTERS` | 3 | Number of sector clusters |
| `SHORT_TARGET` | −0.15 | Short leg target weight (bear regime only) |
| `SMA_SHORT` | 50 | Short SMA for death cross regime filter |
| `SMA_LONG` | 100 | Long SMA for death cross regime filter |
| `LONG_TARGET` | 1.00 | Long leg target weight |
| `FEATURE_WINDOW` | 252 | Lookback window (trading days) for clustering features |
| `MOM_WINDOW` | 126 | 6-month momentum window for cluster labelling |
