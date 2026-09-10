# VoltRadar

**Forecasting Import Growth and Monitoring Trade-Policy Interventions in Lithium-Ion Battery Markets**

## Project Overview

VoltRadar is a machine learning project designed to support international market opportunity assessment for lithium-ion accumulators (HS 850760). It combines historical country-level import data with structured trade-policy intervention data.

The project compares a trade-only forecasting model with a policy-enriched forecasting model to examine whether trade-policy information improves the prediction of future import growth.

## Research Objective

The primary research question is:

> Can historical import data be used to forecast country-level import growth for lithium-ion accumulators, and does the inclusion of trade-policy interventions improve forecasting performance?

## Data Sources

* **Trade Map:** Historical country-level import data for HS 850760
* **Global Trade Alert:** Trade-policy interventions affecting the selected product and markets

Raw datasets are not included in this repository due to potential licensing and redistribution restrictions. Sample or processed data may be provided where permitted.

## Proposed Model Structure

* **Baseline Model:** A simple reference forecast
* **Model 1:** Trade-only import growth forecasting
* **Model 2:** Policy-enriched import growth forecasting
* **Explainability:** Feature importance and SHAP analysis
* **Prototype:** Streamlit-based market opportunity assessment interface

## Repository Structure

* `notebooks/` – Data analysis and machine learning notebooks
* `data/` – Sample data and data documentation
* `src/` – Reusable data processing and modelling functions
* `app/` – Streamlit application
* `models/` – Exported model information
* `outputs/` – Figures, tables and prediction results
* `docs/` – Supporting technical documentation

## Current Status

The project is currently under development as part of a master’s dissertation in Data Science, AI and Digital Business.
