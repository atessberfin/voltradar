# VoltRadar

**Forecasting Import Growth and Monitoring Trade-Policy Interventions in Lithium-Ion Battery Markets**
## 1. Introduction

VoltRadar is a master's dissertation project focused on forecasting country-level import growth for lithium-ion accumulators (HS 850760) and combining these forecasts with trade-policy information to support international market opportunity assessment.

The project uses historical import data from Trade Map together with structured policy-intervention data from Global Trade Alert (GTA). Its main objective is to examine whether historical trade patterns can be used to forecast future import activity and whether the inclusion of trade-policy information improves forecasting performance.

## 2. Project Overview

VoltRadar is designed as an explainable forecasting and decision-support framework for assessing country-level opportunities in lithium-ion accumulator markets.

The modelling stage compares two approaches:

- **Model 1 – Trade-Only Forecast:** uses historical import behavior only.
- **Model 2 – Policy-Enriched Forecast:** uses the same historical trade features together with Global Trade Alert policy-intervention variables.

Both models use the same Random Forest algorithm so that the effect of adding policy information can be evaluated independently of the forecasting method.

The final VoltRadar framework uses Model 1 as the primary forecasting model. Global Trade Alert information is then applied separately in a rule-based decision-support layer to generate policy direction, risk level, opportunity category and a short explanation for each market.

## 3. Research Questions

The project addresses two main research questions:

1. **Can historical import data be used to forecast country-level import growth for lithium-ion accumulators?**

2. **Does the inclusion of trade-policy intervention variables improve forecasting performance?**

## 4. Product Scope

The analysis focuses on:

**HS 850760 – Lithium-ion accumulators**

VoltRadar is designed as a country-level forecasting framework rather than a single-country study. The objective is to evaluate import-market opportunities across multiple countries using a consistent modelling approach.

Country-specific examples are used only to illustrate model behavior, prediction accuracy and market-assessment outputs.

## 5. Data Sources

VoltRadar uses two primary data sources.

### 5.1. Trade Map

Trade Map provides historical country-level import values for HS 850760.

The main analysis period begins in 2012, while the modelling dataset starts later because lagged and rolling features require several years of historical observations.

### 5.2. Global Trade Alert

Global Trade Alert provides structured trade-policy interventions affecting HS 850760 and the relevant markets.

The policy data are transformed into country-year indicators including:

- New Red Interventions
- New Green Interventions
- New Amber Interventions
- Active Red Interventions
- Active Green Interventions
- Active Amber Interventions

The original raw datasets are not included in this repository due to potential licensing, access and redistribution restrictions.

Further details are available in [`data/README.md`](data/README.md).

## 6. Data Preparation

The Trade Map dataset is transformed from a wide year-based structure into a country-year panel dataset suitable for machine-learning analysis.

The preparation process includes:

- filtering the analysis period
- removing aggregate observations such as `World`
- checking missing values and duplicates
- harmonizing country names between Trade Map and Global Trade Alert
- creating lagged import variables
- calculating annual growth indicators
- generating three-year rolling statistics
- aggregating GTA interventions at country-year level
- merging trade and policy data using `Country` and `Year`
- preparing chronological training, validation and test datasets

The final modelling dataset is structured so that information available in year `t` is used to predict import activity in year `t+1`.

## 7. Feature Engineering

Two feature sets are used in the modelling framework.

### Trade Features

Model 1 uses the following eight trade-related features:

- `Year`
- `Log Import Value`
- `Log Import Lag 1`
- `Log Import Lag 2`
- `Log Import Lag 3`
- `Log Growth 1Y`
- `Rolling Mean 3 Years`
- `Rolling Std 3 Years`

These variables capture the current import level, recent import history, short-term growth momentum, medium-term market level and historical volatility.

The model predicts the log-transformed import value for the following year. The prediction is then transformed back to the original scale and compared with the current year's import value to calculate the expected import growth rate.

### Policy Features

Model 2 uses all trade features from Model 1 and adds six Global Trade Alert variables:

- `New Red Interventions`
- `New Green Interventions`
- `New Amber Interventions`
- `Active Red Interventions`
- `Active Green Interventions`
- `Active Amber Interventions`

These variables represent both newly implemented interventions and the broader active policy environment for each country and year.

Derived indicators such as total intervention counts and net policy scores are used for descriptive analysis but are not included as forecasting features because they are calculated directly from the Red, Green and Amber intervention counts.

## 8. Modelling Framework

A persistence benchmark and several machine-learning algorithms were compared during the model-development stage.

The candidate algorithms included:

- Linear Regression
- Decision Tree
- Random Forest
- Gradient Boosting

Random Forest provided the strongest overall validation performance and was selected for the final modelling stage.

Hyperparameter tuning was then performed for the Random Forest model.

### Baseline

A persistence benchmark is used as a simple reference forecast. It assumes that the next year's import level will be similar to the current year's import level.

### Model 1 – Trade-Only Random Forest

Model 1 uses only historical trade-related features.

Its purpose is to evaluate whether country-level import behaviour contains enough information to forecast the following year's import activity.

### Model 2 – Policy-Enriched Random Forest

Model 2 uses the same Random Forest structure and the same trade-related features as Model 1, but adds Global Trade Alert policy variables.

Using the same algorithm for both models makes it possible to evaluate the incremental contribution of policy information without introducing differences caused by the modelling method itself.

Model 2 is retained as an experimental comparison, while Model 1 is used as the final production forecasting model.

## 9. Time-Based Validation

Because VoltRadar is designed to forecast future import activity, the modelling process uses a chronological validation strategy instead of a random train-test split.

The main experimental structure is:

- **Training feature years:** 2015–2021
- **Validation feature year:** 2022
- **Validation target year:** 2023
- **Test feature years:** 2023–2024
- **Test target years:** 2024–2025

This structure ensures that future observations are not used to predict earlier periods and provides a more realistic evaluation of forecasting performance.

An additional expanding-window temporal robustness analysis was also performed across several validation years to check whether the main results remained consistent over time.

## 10. Model Evaluation

Model performance is evaluated primarily using:

- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- R²

Additional growth-oriented diagnostics are also used to understand how well the models capture the direction and magnitude of expected import growth.

The final test results are:

| Model | RMSE | MAE | R² |
|---|---:|---:|---:|
| Persistence Baseline | 1.1563 | 0.7289 | 0.8546 |
| Model 1 – Trade-Only | 1.0101 | 0.6731 | 0.8890 |
| Model 2 – Policy-Enriched | 1.0099 | 0.6782 | 0.8891 |

Both machine-learning models outperform the persistence baseline.

Model 2 achieves only a very small improvement in RMSE and R² compared with Model 1, while Model 1 performs slightly better in MAE. Overall, the results indicate that the policy-enriched feature set does not provide a material improvement in next-year import forecasting performance.

## 11. Growth-Oriented Evaluation

Because the final business output is expressed as expected import growth, additional diagnostics are used alongside the main log-scale evaluation metrics.

The growth-oriented results are:

| Metric | Model 1 – Trade-Only | Model 2 – Policy-Enriched |
|---|---:|---:|
| Growth MAE | 194.71 percentage points | 195.65 percentage points |
| Median Absolute Growth Error | 53.19 percentage points | 49.69 percentage points |
| Growth Direction Accuracy | 69.57% | 67.87% |

A total of 414 observations were evaluated. Six observations were excluded because the current import value was zero and a meaningful percentage growth rate could not be calculated.

The relatively high mean growth errors are largely driven by countries with very small current import values, where even modest absolute changes can produce extremely large percentage movements.

For this reason, RMSE, MAE and R² on the log-transformed import target remain the primary model-selection metrics, while the growth-based measures are treated as complementary diagnostics.

## 12. Temporal Robustness

To test whether the main modelling results remain stable across different time periods, an expanding-window temporal robustness analysis was performed.

Validation years included:

- 2019
- 2020
- 2021
- 2022

For each validation year, the models were trained only on observations from earlier years and then evaluated on the following forecast period.

Average performance across the temporal validation folds was:

| Model | RMSE | MAE | R² |
|---|---:|---:|---:|
| Persistence Baseline | 1.5028 | 0.8682 | 0.8136 |
| Model 1 – Trade-Only | 1.2902 | 0.7917 | 0.8624 |
| Model 2 – Policy-Enriched | 1.2885 | 0.7983 | 0.8628 |

Both Random Forest models consistently outperform the persistence benchmark across the validation periods.

The difference between Model 1 and Model 2 remains very small: Model 2 achieves a slightly lower average RMSE and slightly higher R², while Model 1 produces a slightly lower MAE. This supports the main conclusion that adding the current GTA policy features does not materially change forecasting performance.

## 13. Model Interpretation

Model interpretation was performed using both Random Forest feature importance and SHAP analysis.

### 13.1. Feature Importance

For Model 1, the most influential variables were:

- `Log Import Value`
- `Rolling Mean 3 Years`
- `Log Import Lag 1`
- `Log Import Lag 2`
- `Rolling Std 3 Years`

This indicates that the forecast is driven mainly by the current import level, recent import history and medium-term market behaviour.

For Model 2, historical trade variables remained dominant. Trade-related features accounted for approximately **94.3%** of total feature importance, while policy-related variables accounted for approximately **5.7%**.

Among the policy variables, `Active Green Interventions` and `Active Red Interventions` contributed more than the other GTA features, although their overall influence remained limited.

### 13.2. SHAP Analysis

SHAP analysis was used to examine both the magnitude and direction of feature contributions.

The results confirmed that current import value, recent import history and the three-year rolling mean were the main drivers of model predictions.

Policy variables generally had smaller SHAP effects and were concentrated closer to zero, supporting the conclusion that the current GTA policy features provide limited additional forecasting value.

SHAP results are interpreted as model associations rather than causal relationships.

## 14. Country-Level Analysis

In addition to aggregate evaluation metrics, country-level prediction errors were examined to understand where the models perform well and where forecasting remains difficult.

The analysis shows that prediction accuracy varies considerably across markets.

Countries with more stable and established import patterns generally produce lower prediction errors, while smaller or more volatile markets can be more difficult to forecast.

Representative examples were selected from larger markets to illustrate different levels of model performance:

- **Türkiye** – relatively low prediction error
- **United Arab Emirates** – medium prediction error
- **Uzbekistan** – comparatively high prediction error

These examples show that strong aggregate model performance does not guarantee equally accurate forecasts for every country.

Country-level analysis is therefore used as a complementary evaluation step alongside RMSE, MAE and R².

## 15. Final Model Selection

The **Trade-Only Random Forest** was selected as the final production forecasting model.

This decision was based on several factors:

- it clearly outperformed the persistence baseline
- its performance was almost identical to the Policy-Enriched model
- it showed stable results across the temporal robustness checks
- it requires fewer input features
- it provides a cleaner separation between forecasting and policy interpretation

The Policy-Enriched Random Forest remains an important experimental model because it directly addresses the second research question. However, the results show that the current GTA policy features add only limited incremental predictive value.

For this reason, the final VoltRadar architecture uses Model 1 for forecasting and applies GTA policy information separately in the decision-support layer.

## 16. 2026 Forecast

After the evaluation stage was completed, the final models were retrained using all available labelled historical observations.

The final training dataset covers:

- **Feature years:** 2015–2024
- **Target years:** 2016–2025
- **Training observations:** 2,066

The latest available 2025 features were then used to generate genuine out-of-sample forecasts for 2026.

Forecasts were produced for **211 countries**.

For each country, the framework generates:

- predicted 2026 import value
- predicted import growth rate
- policy direction
- risk level
- opportunity category
- a short market-assessment explanation

The 2026 forecasts cannot yet be evaluated against actual outcomes because the corresponding observed import values are not available. They should therefore be interpreted as forward-looking model estimates rather than validated results.

## 17. Final VoltRadar Decision-Support Framework

The final VoltRadar architecture separates forecasting from policy assessment.

The forecasting component uses the Trade-Only Random Forest to estimate the next year's import value and derive the expected import growth rate.

Global Trade Alert information is then evaluated separately through a transparent policy-assessment layer.

The final workflow is:

```text
Historical Trade Data
        |
        v
Trade-Only Random Forest
        |
        v
Predicted 2026 Import Value
        |
        v
Predicted Import Growth
        |
        +----------------------+
                               |
Global Trade Alert             |
        |                      |
        v                      |
Active Policy Interventions    |
        |                      |
        v                      |
Policy Direction               |
        |                      |
        v                      |
Risk Level                     |
        |                      |
        +----------+-----------+
                   |
                   v
          Opportunity Category
                   |
                   v
          Prediction Explanation
```

This structure was chosen to avoid double-counting policy information in the final decision-support process.

Although Model 2 includes GTA variables directly in the forecasting model, the final framework uses Model 1 for forecasting and applies GTA information only once, in the policy-assessment layer.

## 18. Policy and Opportunity Assessment

The policy layer converts active Global Trade Alert interventions into a simple market-risk indicator.

### Policy Direction

Policy direction is determined by comparing active Green and Red interventions:

```text
Active Green > Active Red  -> Positive
Active Green < Active Red  -> Negative
Active Green = Active Red  -> Neutral
```

The corresponding risk levels are:

```text
Positive -> Low Risk
Neutral  -> Medium Risk
Negative -> High Risk
```

### Opportunity Categories

Predicted import growth and policy direction are then combined into an interpretable opportunity category.

| Forecast Signal | Policy Direction | Opportunity Category |
|---|---|---|
| Growing | Positive | High opportunity |
| Growing | Negative | Growing market with caution |
| Weak or Declining | Negative | High risk |
| Weak or Declining | Positive | Potential future opportunity |
| Any direction | Neutral | Neutral opportunity |

This layer is rule-based rather than a separate machine-learning model. Its purpose is to make the final forecast easier to interpret from a market-assessment perspective.

The policy indicators should therefore be treated as decision-support signals rather than definitive measures of country risk.

## 19. Example Output

The final VoltRadar output provides a country-level summary that combines the forecast result with the policy and opportunity assessment.

Example:

```text
Country: Türkiye

2025 Import Value: 1,778,772

Predicted 2026 Import Value: 1,988,150.53

Predicted Import Growth: +11.77%

Active Red Interventions: 23
Active Green Interventions: 5
Active Amber Interventions: 0

Policy Direction: Negative

Risk Level: High

Opportunity Category: Growing market with caution

Prediction Explanation:
Import demand is expected to grow by approximately 11.8% in 2026,
but the active policy environment is predominantly restrictive,
indicating additional market risk.
```

This output is designed to translate the model forecast into a more interpretable country-level market assessment.

## 20. Repository Structure

The current repository structure is:

```text
voltradar/
|
|-- README.md
|-- requirements.txt
|-- .gitignore
|
|-- notebooks/
|   `-- VoltRadar_Masters_Thesis.ipynb
|
|-- data/
|   `-- README.md
|
|-- models/
|   `-- README.md
|
|-- outputs/
|   `-- README.md
|
`-- app/
    `-- README.md
```

The `notebooks` folder contains the complete analytical workflow used for the dissertation.

The `data` folder documents the project data sources and data-availability restrictions.

The `models` folder documents the exported production model and model artefact policy.

The `outputs` folder documents the generated 2026 market-assessment outputs.

The `app` folder is reserved for the planned interactive VoltRadar prototype.

Raw datasets, exported model artefacts and generated output files are not stored in the repository.

## 21. Notebook Structure

The main notebook contains the complete analytical workflow used in the dissertation.

Its structure is:

```text
1. Business Problem Understanding
2. Data Collection and Understanding
3. Exploratory Data Analysis
4. Data Cleaning and Preprocessing
5. Feature Engineering and Data Integration
6. Model Training
7. Model Evaluation and Comparison
8. Interpretation and Visualisation
9. 2026 Forecast and Final Market Assessment
10. Model Export and Prototype Preparation
11. Conclusion
```

The notebook is organized so that the full process can be followed from raw data preparation through model development, evaluation, interpretation and final market assessment.

## 22. Reproducibility

The project is designed so that the modelling workflow can be reproduced from the analysis notebook.

Required Python packages are listed in:

```text
requirements.txt
```

They can be installed using:

```bash
pip install -r requirements.txt
```

The main dependencies include:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
openpyxl
shap
joblib
```

A fixed random state is used during model development to improve reproducibility of the machine-learning results.

Because the original Trade Map and Global Trade Alert datasets are not distributed in this repository, full reproduction requires authorised access to the original source data.

## 23. Model and Output Export

The final production model can be exported for reuse outside the notebook.

The export stage includes:

- the trained Trade-Only Random Forest model
- the corresponding production feature list
- the final 2026 market-assessment dataset

The model is saved using `joblib`, while the structured market-assessment output is exported as a CSV file.

The notebook also includes a reload check to confirm that the exported model can be loaded successfully with the expected feature configuration.

Generated model and output files are currently kept outside the GitHub repository until licensing, redistribution and prototype requirements are finalised.

## 24. Prediction Function

A reusable country-level prediction function was developed to reproduce the final VoltRadar assessment outside the main notebook workflow.

The function takes a selected country and:

- retrieves the latest available trade features
- predicts the 2026 import value using the final Trade-Only Random Forest
- calculates the expected import growth rate
- retrieves the active GTA intervention counts
- determines policy direction
- assigns a risk level
- assigns an opportunity category
- generates a short prediction explanation

The function returns a structured one-row output containing the main forecast and decision-support indicators for the selected market.

This function serves as the technical core for the planned interactive prototype.

## 25. Prototype Preparation

The final notebook includes the technical components required for a future interactive prototype.

The planned prototype will use the reusable country-level prediction function to allow a user to select a market and view:

- the latest available import value
- the predicted 2026 import value
- the predicted import growth rate
- active Red, Green and Amber policy interventions
- policy direction
- risk level
- opportunity category
- a short market explanation

A lightweight web application is planned for the prototype stage so that the final framework can be explored without interacting directly with the notebook.

Streamlit is currently the preferred option because it can integrate directly with the Python forecasting pipeline and exported model components.

## 26. Limitations

The current VoltRadar framework has several limitations.

The forecasting models depend heavily on historical trade behaviour. As a result, performance can be weaker in markets with sudden structural changes, irregular import patterns or very high volatility.

Percentage-growth measures can also become unstable when the current import value is very small. For this reason, the main models are trained and evaluated using the log-transformed import target, while growth-based metrics are treated as complementary diagnostics.

The Global Trade Alert variables are aggregated into intervention counts. These features indicate the presence and direction of policy activity but do not capture the full economic size, legal intensity or practical market impact of individual interventions.

The final policy-risk layer is rule-based and should therefore be interpreted as a decision-support indicator rather than a definitive measure of market risk.

Finally, the current analysis focuses specifically on HS 850760 and does not yet incorporate wider macroeconomic variables, firm-level indicators or information from related product markets.

## 27. Future Development

Several extensions could improve VoltRadar in future versions.

Potential developments include:

- an interactive Streamlit web application
- richer representations of Global Trade Alert interventions
- additional macroeconomic indicators
- country-level uncertainty estimates
- broader HS product coverage
- interactive historical trend visualisations
- downloadable country-level market reports
- automated model retraining when new trade data become available

The current framework therefore provides a foundation that can be extended from an academic forecasting project into a more practical market-intelligence application.

## 28. Current Status

The core machine-learning pipeline is complete.

Completed work includes:

- data preparation and cleaning
- exploratory data analysis
- country-name harmonisation
- feature engineering
- Trade Map and Global Trade Alert integration
- persistence baseline benchmarking
- candidate model comparison
- Random Forest hyperparameter tuning
- Trade-Only and Policy-Enriched model evaluation
- growth-oriented diagnostics
- temporal robustness analysis
- feature importance analysis
- SHAP interpretation
- country-level evaluation
- 2026 forecasting
- policy-risk assessment
- market opportunity categorisation
- prediction explanations
- final production-model selection
- model and data export logic
- reusable country-level prediction function

The next project stages are:

**written dissertation → prototype development → viva presentation**

## 29. Academic Context

VoltRadar was developed as part of a master's dissertation in Data Science, AI and Digital Business.

The repository provides the technical implementation supporting the dissertation's:

- data preparation and feature engineering
- forecasting experiments
- model comparison and evaluation
- temporal robustness analysis
- explainability work
- 2026 market forecasts
- policy-risk assessment
- final decision-support framework

The notebook and repository are intended to support transparency, reproducibility and traceability between the technical implementation and the written dissertation.
