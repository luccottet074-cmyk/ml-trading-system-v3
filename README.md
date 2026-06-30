# ML Trading System — V3

A machine learning–driven quantitative trading pipeline for US equities, combining walk-forward validated predictive models with risk-managed live execution via Interactive Brokers (paper trading).

## Overview

This system predicts short-horizon returns on a basket of large-cap US equities (AAPL, MSFT, GOOGL, COST) using an ensemble of ML models (Ridge regression, XGBoost), trained on 90+ engineered alpha factors including price action, volatility (implied & realized), and intraday microstructure signals.

## Architecture

Data Ingestion (IBKR)
│
▼
Feature Engineering (90 alpha factors)
│
▼
Model Training (Ridge / XGBoost) ── Walk-Forward Validation (5 folds)
│
▼
Signal Generation ── Kelly-weighted Position Sizing ── Regime Filter
│
▼
Risk Management (macro-event blackout, volatility scaling, drawdown control)
│
▼
Live Execution (IBKR API) ── Automated Monitoring & Alerting



## Key Features

- **Walk-forward validation**: 5-fold rolling validation to prevent lookahead bias, robustness-checked before every deployment
- **Multi-model ensemble**: per-ticker model selection (Ridge / XGBoost) based on out-of-sample Information Coefficient
- **Risk-aware position sizing**: Kelly-weighted allocation scaled by model confidence and realized/implied volatility
- **Macro-event protection**: automated blackout windows around FOMC, CPI, PPI, PCE/GDP, and NFP releases
- **Regime detection**: dynamically blocks new entries during adverse market regimes
- **Explainability**: SHAP-based feature attribution tracked per ticker
- **Live IC monitoring**: rolling Information Coefficient tracking with automated degradation alerts
- **Automated execution**: scheduled live pipeline with SMS alerting and end-of-day risk closeout

## Performance Snapshot (Backtest, latest validation)

*Backtested on ~5 years of hourly market data (2021–2026).*

| Metric | Value |

| Metric | Value |
|---|---|
| Cumulative return (strategy) | +40.7% |
| Cumulative return (Buy & Hold) | +26.2% |
| Sharpe Ratio | 6.07 |
| Sortino Ratio | 10.97 |
| Max Drawdown | -1.0% |
| Deflated Sharpe Ratio | 100% |
| Walk-forward robustness | 5/5 folds passed |

*Currently running in live paper trading on Interactive Brokers for ongoing real-market validation.*

## Tech Stack

`Python` · `scikit-learn` · `XGBoost` · `pandas` / `numpy` · `ib_insync` (Interactive Brokers API) · `SHAP`

## Disclaimer

This project is for educational and research purposes only. It currently runs exclusively in **paper trading** (simulated capital, no real funds). Nothing here constitutes financial advice. Proprietary feature engineering, model internals, and execution logic are not published in this repository.
