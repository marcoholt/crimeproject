# Urban Safety Risk Forecasting for Real Estate Investment

## Overview

Real estate investment decisions are heavily influenced by neighborhood safety. Crime trends can affect property values, rental demand, insurance costs, tenant retention, and long-term investment performance. Investors often rely on historical observations and subjective assessments when evaluating neighborhoods, which can lead to missed opportunities or increased risk exposure.

This project develops a machine learning forecasting solution that predicts future neighborhood crime activity in Los Angeles using historical crime data. The goal is to provide data-driven insights that help real estate investors identify safer investment opportunities and avoid neighborhoods exhibiting increasing crime risk.

---

## Business Problem

### Problem Statement

Morro's Real Estate Investment Firm wants to make more informed investment decisions by understanding which Los Angeles neighborhoods are becoming safer or riskier over time.

Crime activity directly impacts:

- Property values
- Rental demand
- Tenant retention
- Insurance costs
- Long-term investment returns
- Overall neighborhood desirability

Without predictive insights, investment decisions may rely on historical observations rather than future risk forecasts.

### Business Question

> Can historical crime data be used to forecast future neighborhood crime activity and support real estate investment decisions?

---

## Business Value

This project helps investors:

### Identify Lower-Risk Investment Opportunities

Forecasting future crime activity allows investors to prioritize neighborhoods expected to remain stable or improve over time.

### Reduce Investment Risk

Neighborhoods with increasing crime trends may experience declining property values and lower tenant demand.

### Support Data-Driven Decisions

Rather than relying on intuition, investors can leverage historical patterns and predictive analytics when evaluating investment opportunities.

### Improve Portfolio Performance

Allocating capital toward lower-risk neighborhoods can potentially improve long-term portfolio returns.

---

# Dataset

## Data Source

### Los Angeles Crime Data (2020–2024)

Publicly available crime incident data containing reported crimes across Los Angeles neighborhoods.

Key fields include:

- Crime occurrence date
- Neighborhood (Area Name)
- Crime classification
- Victim demographics
- Location information
- Time information

---

## Data Preparation

### Initial Cleaning

The following steps were performed:

- Removed duplicate records
- Checked for missing values
- Converted date fields into datetime format
- Standardized column names
- Removed irrelevant identifiers and administrative fields

Removed fields included:

```text
DR_NO
LOCATION
Cross Street
Crm Cd 2
Crm Cd 3
Crm Cd 4
Status
Status Desc
```

These fields were excluded because they provided little predictive value for neighborhood-level crime forecasting.

---

## Time Feature Creation

Several date-based features were extracted:

```text
Year
Month
DayOfWeek
Hour
```

These features help capture temporal crime patterns.

---

# Exploratory Data Analysis

Several analyses were conducted to understand crime patterns across time and geography.

### Annual Crime Trends

Crime activity varied across years, revealing meaningful changes in neighborhood crime behavior.

### Monthly Trends

Crime activity exhibited seasonal fluctuations throughout the year.

### Neighborhood Analysis

Significant differences in crime volume were observed between neighborhoods.

### Temporal Patterns

Crime frequency varied across:

- Months
- Days of the week
- Hours of the day

These findings supported the inclusion of temporal forecasting features.

---

# Feature Engineering

A neighborhood-month level forecasting dataset was created by aggregating crime incidents.

### Target Variable

```text
crime_count
```

Represents the total number of crimes occurring within a neighborhood during a given month.

---

## Forecasting Features

### previous_crime_count

Captures the prior month's crime activity and provides immediate historical context.

### rolling_average

Calculates the average crime count over the previous three months.

Helps smooth short-term fluctuations and identify persistent patterns.

### crime_trend

Measures month-to-month changes in crime activity.

Allows the model to detect increasing or decreasing crime trends.

---

## Additional Feature Engineering

To improve forecasting performance and model generalization, several additional temporal features were engineered.

### Lag Features

```text
lag_1
lag_3
lag_6
lag_12
```

These features capture crime activity from previous months and allow the model to learn short-term and long-term historical dependencies.

### Rolling Averages

```text
rolling_3
rolling_6
rolling_12
```

These features smooth random fluctuations and reveal underlying crime trends.

### Cyclical Seasonality Features

```text
month_sin
month_cos
```

These features allow the model to understand that months occur in a repeating annual cycle.

For example:

```text
December and January are close together
```

rather than being numerically distant.

---

# Modeling Approach

## Forecasting Strategy

Unlike traditional random train-test splits, forecasting requires preserving chronological order.

### Training Data

```text
2020
2021
2022
2023
```

### Testing Data

```text
2024
```

This simulates a real-world forecasting scenario where future data is unavailable during training.

---

# Machine Learning Pipeline

A reproducible Scikit-Learn pipeline was implemented using:

```python
ColumnTransformer
Pipeline
SimpleImputer
StandardScaler
OneHotEncoder
```

Benefits:

- Consistent preprocessing
- Reduced data leakage
- Reproducibility
- Production-ready workflow

---

# Models Evaluated

## Linear Regression

Baseline forecasting model.

---

## Ridge Regression

Regularized linear regression model designed to reduce overfitting and improve generalization.

---

## Random Forest Regressor

Ensemble tree-based model capable of capturing nonlinear relationships.

---

## XGBoost Regressor

Gradient boosting model optimized for predictive performance.

---

## Traditional Forecasting Models

### SARIMA

Evaluated to determine whether classical time-series forecasting could outperform machine learning models.

### Prophet

Evaluated to compare modern forecasting methods against feature-engineered machine learning approaches.

---

# Hyperparameter Tuning

Hyperparameter optimization was performed using:

```python
GridSearchCV
TimeSeriesSplit
```

This ensured model tuning respected chronological ordering and prevented future data leakage.

---

# Evaluation Metrics

The following metrics were used:

## MAE (Mean Absolute Error)

Measures average prediction error.

Lower values indicate better performance.

---

## RMSE (Root Mean Squared Error)

Penalizes larger prediction errors.

Lower values indicate better performance.

---

## R² (Coefficient of Determination)

Measures the percentage of variation explained by the model.

Higher values indicate better predictive performance.

---

# Results

## Best Performing Model

### Ridge Regression

Final Performance:

```text
R² = 0.51
```

This means the model explains approximately:

```text
51% of future neighborhood crime variation
```

using historical crime patterns.

---

# Model Comparison


| Model            | R²    |
| ---------------- | ----- |
| Ridge Regression | 0.51  |
| Random Forest    | 0.01  |
| XGBoost          | -0.12 |
| SARIMA           | -4.70 |
| Prophet          | -4.98 |


---

# Key Findings

## 1. Simpler Models Generalized Better

Despite testing more complex models, Ridge Regression significantly outperformed Random Forest and XGBoost on unseen future data.

This suggests neighborhood crime activity is driven primarily by stable historical patterns rather than highly complex nonlinear relationships.

---

## 2. Feature Engineering Was Critical

Additional forecasting features improved Ridge Regression performance from:

```text
R² = 0.44
```

to:

```text
R² = 0.51
```

demonstrating the importance of capturing seasonality and long-term historical trends.

---

## 3. Traditional Forecasting Models Performed Poorly

Both SARIMA and Prophet produced negative R² scores.

This indicates that neighborhood-level crime forecasting benefits from engineered temporal features and neighborhood information rather than relying solely on historical crime counts.

---

## 4. Historical Crime Activity Is Predictive

Features such as:

```text
lag_12
rolling_12
crime_trend
```

provided valuable information for forecasting future crime activity.

---

# Translating Model Performance into Business Value

An R² of 0.51 does not mean predictions are perfect.

However, it indicates the model can explain over half of the variation in future neighborhood crime activity.

For investors, this creates actionable value by:

### Identifying Higher-Risk Neighborhoods

Areas exhibiting rising crime trends can be flagged for additional investigation.

### Prioritizing Safer Opportunities

Neighborhoods with stable or declining crime forecasts may represent stronger investment candidates.

### Supporting Capital Allocation Decisions

Forecasted crime trends can become an additional factor within investment evaluation frameworks.

### Reducing Subjectivity

Investment decisions become increasingly data-driven rather than intuition-based.

---

# Recommendations

## Short-Term

Incorporate forecasted crime trends into neighborhood screening processes.

Use crime forecasts alongside:

- Property values
- Rental yields
- Vacancy rates
- Population growth

---

## Medium-Term

Develop neighborhood risk scores based on:

```text
Predicted Crime Level
Crime Trend
Historical Volatility
```

to simplify investment decision-making.

---

## Long-Term

Create an automated forecasting dashboard that updates monthly as new crime data becomes available.

Potential enhancements include:

- Interactive maps
- Neighborhood rankings
- Risk alerts
- Portfolio-level risk analysis

---

# Limitations

## Data Quality

The source crime dataset is publicly available and may contain:

- Reporting delays
- Missing incidents
- Data revisions

These factors can impact forecasting performance.

---

## External Factors

Crime activity is influenced by many variables not included in the dataset:

- Economic conditions
- Housing markets
- Policing strategies
- Population shifts
- Policy changes

---

## Forecast Horizon

The model was evaluated using a one-year forecasting horizon.

Performance may vary over longer forecasting periods.

---

## Geographic Scope

Results are specific to Los Angeles neighborhoods and may not generalize to other cities without retraining.

---

# Future Improvements

Potential future enhancements include:

### Additional Data Sources

- Economic indicators
- Housing market data
- Population demographics
- Employment statistics

### Advanced Forecasting Approaches

- Global forecasting models
- Temporal Fusion Transformers
- LSTM Neural Networks
- LightGBM Forecasting

### Explainability

- SHAP values
- Feature importance analysis
- Neighborhood risk explanations

---

# Conclusion

This project demonstrates that historical crime data can be used to forecast future neighborhood crime activity and support real estate investment decision-making. Through extensive feature engineering, time-aware validation, and model comparison, Ridge Regression emerged as the strongest forecasting model, explaining approximately 51% of future crime variation across Los Angeles neighborhoods.

The results suggest that feature-engineered machine learning approaches outperform traditional forecasting models for neighborhood-level crime prediction and can provide valuable insights for identifying safer investment opportunities, reducing risk exposure, and supporting data-driven real estate investment strategies.