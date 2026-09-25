# Outputs

This folder contains selected generated outputs from the final VoltRadar forecasting and market-assessment workflow.

The primary final output is the **2026 country-level market assessment**, generated after the production Trade-Only Random Forest model was retrained using all labelled observations available up to 2025.

The final assessment covers **211 markets**.

For each country, the output includes:

- 2025 import value
- predicted 2026 import value
- predicted import growth rate
- policy direction
- risk level
- opportunity category
- prediction explanation

## Available Output Files

### `voltradar_2026_market_assessment.csv`

This file contains the complete machine-readable 2026 country-level production output.

It includes the full set of country-level forecasting and decision-support variables, including the prediction explanation generated for each market.

### `voltradar_appendix_c_2026_market_assessment.xlsx`

This file contains a simplified and formatted version of the 2026 country-level assessment prepared to support Appendix C of the written dissertation.

The Excel output focuses on the main variables required for presentation:

- Country
- 2025 Import Value
- Predicted 2026 Import Value
- Predicted Growth (%)
- Policy Direction
- Risk Level
- Opportunity Category

## Interpretation

The 2026 forecasts are forward-looking model estimates because observed 2026 import values are not yet available.

Policy direction, risk level and opportunity category are generated through the separate rule-based policy-assessment layer. These indicators are designed for decision-support purposes and should not be interpreted as causal estimates or comprehensive measures of country risk.

## Reproducibility

Both output files are generated from the final workflow contained in:

`notebooks/VoltRadar_Masters_Thesis.ipynb`

The outputs retained in this directory provide a direct record of the final results used in the dissertation and support transparency and reproducibility of the VoltRadar framework.
