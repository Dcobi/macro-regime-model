# Macro Regime Allocation Model

A multi-asset allocation model that classifies the US and Euro area economies into four macro regimes and tests whether tilting a portfolio by regime improves on a static allocation, with realistic publication lags and trading costs.

**Status:** work in progress

## Question

Macro regimes are easy to identify in hindsight. Can they be identified **in real time**, early enough to improve a portfolio?

## Method

1. **Regimes:** monthly growth (industrial production) and inflation (CPI / HICP) for the US and the Euro area. Each is classified as rising or falling against its 12-month average, giving four regimes: Goldilocks, Overheating, Slowdown and Stagflation.
2. **Asset behaviour:** returns of seven asset classes (US and European equities, long and short US Treasuries, investment-grade credit, gold, commodities) in each regime, in USD and EUR.
3. **Backtest:** a euro investor tilts a diversified neutral portfolio by regime, using only data that would have been published at the time, after trading costs. Compared with the neutral portfolio and a 60/40 portfolio.

## Key findings (2006–2026, euro investor)

- **Regimes contain information, but the edge is fragile.** Using economic sentiment for Euro area growth, regime tilts added about 1 percentage point a year over a static diversified portfolio after realistic publication lags and trading costs. However, the gain came almost entirely from 2006–2015 (mainly 2008); since 2016 the strategy has not outperformed.
- **Better data beats faster data.** Replacing industrial production with the Economic Sentiment Indicator raised the edge from 0.3 to about 1 point a year, and results were similar whether the signal was delayed by 1 or 4 months.
- **Diversification is the robust result.** A static portfolio of seven asset classes beat 60/40 on a risk-adjusted basis in both halves of the sample, with a much smaller worst loss (about −16% vs −25%).


## Structure

```
data/         Saved datasets (macro data, regimes, asset returns)
notebooks/
  01_data_pipeline.ipynb   Macro data download and regime classification
  02_asset_returns.ipynb   Asset returns by regime, USD vs EUR
  03_backtest.ipynb        Backtest with publication lags and trading costs
```

## Data sources

FRED (US CPI, industrial production, EUR/USD), ECB Data Portal (Euro area HICP and industrial production), Yahoo Finance via `yfinance` (ETF prices).

## How to run

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Then run the notebooks in order: 01 → 02 → 03.

## Possible extensions

- Threshold rule to reduce switching in flat periods
- US / dollar version of the backtest
- Sensitivity to higher trading costs