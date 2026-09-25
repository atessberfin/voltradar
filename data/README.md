# Data

VoltRadar uses two primary data sources:

- **Trade Map** – historical country-level import data for HS 850760 (lithium-ion accumulators).
- **Global Trade Alert (GTA)** – trade-policy interventions affecting the selected product and markets.

## Data Coverage

The main descriptive analysis covers the period from 2012 to 2025.

The model-ready dataset begins later because the forecasting framework requires lagged and rolling historical features. Country-level observations are organized in a country-year structure before modelling.

The 2026 forecasts are generated using the latest available 2025 trade information together with the engineered historical features.

## Data Availability

The original Trade Map and Global Trade Alert datasets are not redistributed through this repository.

The source datasets can be obtained from their original providers:

- Trade Map: https://www.trademap.org/
- Global Trade Alert: https://globaltradealert.org/

The analysis notebook documents the data-processing workflow used in VoltRadar, including:

- data cleaning
- filtering of the analysis period
- removal of aggregate observations such as `World`
- country-name harmonization
- transformation to country-year format
- lagged and rolling feature engineering
- Global Trade Alert intervention aggregation
- trade and policy data integration
- chronological modelling-dataset preparation

## Reproducibility

To reproduce the complete workflow, the relevant source datasets must first be obtained from the original providers and prepared according to the steps documented in the main analysis notebook.

Raw source datasets are intentionally excluded from this repository. Selected derived forecasting and market-assessment outputs used in the dissertation are retained in the `outputs/` directory.
