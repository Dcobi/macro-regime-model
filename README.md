# Macro Regime Allocation Model

A multi-asset allocation model that classifies the US and Euro area economies into four macro regimes and tests whether tilting a portfolio by regime improves on a static allocation, with realistic publication lags and trading costs.

**Status:** work in progress (analysis complete; current-regime notebook and written report in progress). Results use data downloaded on 3 October 2026.

## Question

Macro regimes are easy to identify in hindsight. Can they be identified **in real time**, early enough to improve a portfolio?

## Method

1. **Regimes:** each month, growth and inflation are classified as rising or falling against their 12-month average, giving four regimes: Goldilocks, Overheating, Slowdown and Stagflation. US: industrial production and CPI. Euro area: the European Commission's Economic Sentiment Indicator (ESI) and HICP inflation.
2. **Asset behaviour:** returns of seven asset classes (US and European equities, long and short US Treasuries, investment-grade credit, gold, commodities) in each regime, in USD and EUR.
3. **Backtest:** a euro investor tilts a diversified neutral portfolio by regime, using only data that would have been published at the time (1-month lag for the Euro area, 2 months for the US), after trading costs. Tilts are set in advance from economic reasoning, not fitted to the data. Compared with the neutral portfolio and a 60/40 portfolio.
4. **Robustness:** two halves of the sample, lags of 0–4 months, two growth measures, trading costs, a US investor version, a block bootstrap, a permutation test, results without 2008, and the stock–bond correlation by regime.

## Key findings (March 2006 – September 2026, euro investor)

| | Regime strategy | Neutral | 60/40 |
|---|---|---|---|
| Annual return | 8.08% | 7.04% | 6.97% |
| Volatility | 8.55% | 8.03% | 9.74% |
| Return / volatility | 0.95 | 0.88 | 0.72 |
| Max drawdown | −13.5% | −16.3% | −24.5% |

- **Regimes contain some information, but the edge is small and not statistically significant.** The strategy added about 1 percentage point a year over the neutral portfolio, but the block bootstrap 95% interval is [−0.9, 3.0] points and the permutation test gives p ≈ 0.10.
- **The edge is period-dependent.** It comes almost entirely from 2006–2015, mainly 2008; since 2016 the strategy has not beaten the neutral portfolio, and for a US investor the timing reverses. The model helps in gradual downturns and fails after sudden shocks (2020).
- **Timeliness and persistence of the signal matter.** At the same lag, industrial production and the ESI perform similarly, but industrial production is published too late and its information fades quickly; the ESI is available sooner and still works with a delay of several months.
- **Diversification is the robust result.** A static portfolio of seven asset classes had lower volatility and a much smaller worst loss than 60/40 in every test, although in 2022 stocks and bonds fell together.

## Structure

```
data/
  macro_data.csv        Growth and inflation, industrial production version (1997–)
  signal_inputs.csv     Series behind the final regimes (US IP and CPI, EA ESI and HICP)
  regimes.csv           Monthly regimes, US and Euro area
  returns_usd.csv       Monthly ETF returns in USD
  returns_eur.csv       Monthly ETF returns in EUR
  regime_weights.csv    Portfolio weights by regime
notebooks/
  01_data_pipeline.ipynb    Macro data download and regime classification
  02_asset_returns.ipynb    Asset returns by regime, USD vs EUR
  03_backtest.ipynb         Backtest with publication lags, trading costs and robustness tests
  04_current_regime.ipynb   Current regime and implied portfolio (in progress)
```

## Data sources

- **FRED:** US CPI (`CPIAUCSL`), US industrial production (`INDPRO`), EUR/USD (`DEXUSEU`)
- **ECB Data Portal:** Euro area HICP inflation, Euro area industrial production (first version of the model)
- **Eurostat:** Economic Sentiment Indicator, euro area (`ei_bssi_m_r2`, `EA21`)
- **Yahoo Finance** via `yfinance`: ETF prices adjusted for dividends and splits (SPY, VGK, TLT, SHY, LQD, GLD, DBC)

Re-downloading prices can change results very slightly, because adjusted prices are recalculated after each dividend.

## How to run

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Then run the notebooks in order: 01 → 02 → 03 → 04.

## Main limitations

Sample starts in 2006 (excludes the 2000–2002 bear market); revised rather than real-time data; monthly signals react late to sudden shocks; two growth measures and several lags were examined, so the p-value is optimistic.
