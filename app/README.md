# App

This folder is reserved for the interactive VoltRadar prototype.

The planned application will provide a simple country-level interface for exploring the final forecasting and decision-support framework without requiring direct interaction with the analysis notebook.

The prototype is expected to allow users to:

- select a country
- view the latest available import value
- view the predicted 2026 import value
- view the predicted import growth rate
- inspect active Red, Green and Amber policy interventions
- view the derived policy direction
- view the assigned risk level
- view the market opportunity category
- read a short prediction explanation

The application will use the final **Trade-Only Random Forest** as the forecasting engine and apply the Global Trade Alert policy layer separately.

A Streamlit-based implementation is planned for the prototype stage because it can integrate directly with the Python model and prediction workflow developed in the main notebook.

The application code will be added after the core dissertation modelling workflow and repository structure are finalised.
