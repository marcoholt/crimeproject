# Urban Safety Risk Forecasting for Real Estate Investment Decisions

## Project Overview

Real estate investment decisions are heavily influenced by neighborhood safety, crime trends, and long-term community stability. Areas experiencing increasing crime activity may face declining property values, reduced tenant demand, higher insurance costs, and increased investment risk.

Morro's Real Estate Investment Firm wanted a data-driven approach to evaluate crime trends across Los Angeles neighborhoods and identify areas that may become safer or riskier over time.

This project leverages historical crime data from Los Angeles (2020–2024) to forecast future crime activity and provide actionable investment insights. The final solution combines time-aware feature engineering, machine learning forecasting, and business-focused recommendations to support smarter real estate acquisition decisions.

---

## Business Problem

### Challenge

Traditional real estate analysis often relies on historical crime reports without considering future crime trends.

This creates several risks:

- Investing in neighborhoods where crime is increasing.
- Missing opportunities in neighborhoods becoming safer.
- Relying on subjective judgment instead of data-driven forecasts.
- Increased exposure to declining property values and tenant demand.

### Business Question

**Can historical crime data be used to forecast future neighborhood crime activity and identify areas with elevated investment risk?**

### Business Value

This solution helps Morro's:

- Identify safer neighborhoods for investment.
- Avoid neighborhoods experiencing increasing crime activity.
- Prioritize due diligence efforts.
- Improve portfolio risk management.
- Support data-driven acquisition strategies.

---

# Dataset

### Source

Los Angeles Crime Data (2020–2024)

### Unit of Analysis

Monthly crime counts aggregated by neighborhood (Area Name).

### Target Variable

`crime_count`

The total number of crimes reported within a neighborhood during a specific month.

---

# Project Pipeline

## 1. Data Cleaning

The raw crime dataset was cleaned and prepared by:

- Removing duplicate records
- Handling missing values
- Converting dates into datetime format
- Extracting temporal information
- Aggregating crimes into monthly neighborhood-level counts

---

## 2. Feature Engineering

To improve forecasting performance, several time-based features were engineered.

### Previous Crime Count

`previous_crime_count`

Captures crime activity from the prior month.

**Business Value:**

Crime levels often exhibit momentum. Areas experiencing elevated crime today frequently continue experiencing elevated crime in the near future.

---

### Rolling Average

`rolling_average`

Three-month moving average of crime activity.

**Business Value:**

Smooths short-term fluctuations and captures underlying crime patterns.

---

### Crime Trend

`crime_trend`

Month-over-month change in crime volume.

**Business Value:**

Helps identify neighborhoods where crime is increasing or decreasing.

---

### Additional Forecasting Features

Additional lag and seasonality features were engineered:

- lag_1
- lag_3
- lag_6
- lag_12
- rolling_3
- rolling_6
- rolling_12
- month_sin
- month_cos

These features helped the model capture:

- Long-term trends
- Seasonal patterns
- Historical crime momentum
- Neighborhood-specific behavior

---

## Feature Engineering Impact

Additional feature engineering significantly improved model generalization.

### Before Additional Features


| Model            | Holdout R² |
| ---------------- | ---------- |
| Ridge Regression | 0.44       |


### After Additional Features


| Model            | Holdout R² |
| ---------------- | ---------- |
| Ridge Regression | 0.51       |


### Key Finding

The additional lag and seasonality features improved the model's ability to generalize to unseen future data, increasing predictive performance by approximately **16%**.

---

# Modeling Approach

Because the objective was forecasting future crime activity, a chronological train-test split was used.

### Training Period

2020–2023

### Testing Period

2024

This prevents future information from leaking into the training process and better simulates real-world forecasting.

---

## Models Evaluated

### Linear Regression

Baseline regression model.

### Ridge Regression

Regularized version of Linear Regression that reduces overfitting.

### Random Forest Regressor

Tree-based ensemble model.

### XGBoost Regressor

Gradient boosting model.

### SARIMA

Classical time-series forecasting model.

### Prophet

Forecasting model developed by Meta.

---

# Model Evaluation

## Final Holdout-Year Results (2024)


| Model            | MAE    | RMSE   | R²       |
| ---------------- | ------ | ------ | -------- |
| Ridge Regression | 138.14 | 178.64 | **0.51** |
| Random Forest    | 218.80 | 254.83 | 0.01     |
| XGBoost          | 230.58 | 270.75 | -0.12    |
| SARIMA           | 464.55 | 524.62 | -4.70    |
| Prophet          | 486.34 | 539.48 | -4.98    |


---

# Key Findings

### Ridge Regression Outperformed More Complex Models

Despite being the simplest model tested, Ridge Regression generalized substantially better than Random Forest, XGBoost, SARIMA, and Prophet.

This indicates that neighborhood crime patterns were largely driven by linear relationships and historical crime momentum rather than highly complex nonlinear interactions.

---

### Time-Series Models Performed Poorly

SARIMA and Prophet produced extremely poor results.

Possible reasons include:

- Limited history (5 years)
- Structural shifts in crime reporting
- Neighborhood-level variability
- Aggregation effects

This demonstrates that classical forecasting methods are not always superior to machine learning approaches.

---

### Feature Engineering Was Critical

Lag features, rolling averages, trend indicators, and seasonal variables significantly improved performance.

The model's strongest predictor of future crime was often its historical crime activity.

---

# 2025 Neighborhood Crime Forecasts

After selecting Ridge Regression as the final model, it was retrained using all available historical data from 2020–2024 and used to generate neighborhood crime forecasts for 2025.

These forecasts provide Morro's Real Estate Investment Firm with forward-looking risk assessments that can be incorporated into investment decisions.

---

## Highest Predicted Crime Neighborhoods (2025)


| Neighborhood    | Avg Monthly Crime | Annual Forecast |
| --------------- | ----------------- | --------------- |
| Central         | 469.11            | 5,629           |
| Pacific         | 363.55            | 4,363           |
| Southwest       | 289.73            | 3,477           |
| North Hollywood | 249.34            | 2,992           |
| Harbor          | 237.37            | 2,848           |


### Business Interpretation

These neighborhoods are forecasted to experience the highest crime activity in 2025.

This does not necessarily eliminate investment opportunities but suggests increased due diligence should be performed regarding:

- Tenant demand
- Property appreciation potential
- Insurance costs
- Local development activity
- Crime mitigation initiatives

---

## Lowest Predicted Crime Neighborhoods (2025)


| Neighborhood | Avg Monthly Crime | Annual Forecast |
| ------------ | ----------------- | --------------- |
| Foothill     | 108.48            | 1,302           |
| West Valley  | 156.11            | 1,873           |
| Southeast    | 162.49            | 1,950           |
| 77th Street  | 163.69            | 1,964           |
| Mission      | 171.67            | 2,060           |


### Business Interpretation

These neighborhoods are forecasted to experience comparatively lower crime activity in 2025 and may represent attractive investment opportunities when combined with favorable economic and housing indicators.

---

# Translating Model Metrics into Business Value

An R² score of **0.51** means the model explains approximately **51% of future crime variation across neighborhoods**.

While not perfect, this level of predictive power is meaningful in a highly complex social system such as crime forecasting.

For investors, the model provides:

- Early identification of higher-risk neighborhoods
- Relative ranking of neighborhoods by projected risk
- Better-informed acquisition decisions
- Additional context during due diligence

The goal is not perfect prediction but improved decision-making.

---

# Recommendations

### Immediate Recommendations

- Prioritize further investigation of lower-risk neighborhoods.
- Apply enhanced due diligence to higher-risk neighborhoods.
- Combine forecasts with property valuation metrics.
- Incorporate rental demand and vacancy data.

### Future Enhancements

- Include demographic information.
- Add economic indicators.
- Incorporate housing market trends.
- Add unemployment statistics.
- Expand historical coverage beyond 2020.
- Integrate geographic crime hotspot analysis.

---

# Limitations

Several limitations should be considered:

### Data Quality

The Los Angeles crime dataset is preliminary and subject to revision.

### Limited Historical Window

Only five years of historical data were available.

### External Factors

Crime is influenced by factors not included in this dataset:

- Economic conditions
- Population shifts
- Policy changes
- Law enforcement strategies

### Forecast Uncertainty

Predictions represent expected trends rather than guaranteed outcomes.

---

# Repository Structure

```
crimeproject/
│
├── .gitignore
├── README.md
├── crime.ipynb
└── requirements.txt
```

---

# Installation & Setup

### Clone Repository

```bash
git clone https://github.com/marcoholt/crimeproject.git
cd crimeproject
```

### Create Virtual Environment

```bash
python -m venv venv
```

Activate:

Mac/Linux

```bash
source venv/bin/activate
```

Windows

```bash
venv\Scripts\activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Launch Notebook

```bash
jupyter notebook
```

Open:

```text
crime_data_project/crime.ipynb
```

---

# Conclusion

This project demonstrates how machine learning can be used to support real estate investment decisions through crime forecasting. By combining historical crime patterns, advanced feature engineering, and predictive modeling, the final Ridge Regression model successfully identified neighborhood-level risk patterns and generated actionable 2025 crime forecasts.

The resulting forecasts provide Morro's Real Estate Investment Firm with a scalable, data-driven framework for evaluating neighborhood safety and incorporating crime risk into future investment decisions.