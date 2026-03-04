# Crop Yield Forecasting System

**Time-Aware Multivariate Forecasting with Uncertainty Quantification**

A production-ready machine learning system for forecasting agricultural crop yields using historical data, climate variables, and autoregressive time-series features.

The model achieves:

* **RMSE:** 3,400 hg/ha
* **MAPE:** 1.47%
* **R²:** 0.985
* **72% error reduction over naive baseline**

---

## Project Overview

This system predicts crop yield across 50+ countries and 15+ crop types using:

* Historical yield trends
* Climate variables (rainfall, temperature)
* Engineered lag and rolling features
* Climate anomaly detection
* Quantile regression for uncertainty estimation

The objective is to support food security planning, agricultural optimization, and risk-aware decision-making.

---

## Dataset

* 25,854 records
* Training: 1990–2012
* Testing: 2013–2016
* 7 raw features expanded into 20+ engineered features

---

## Feature Engineering

Key feature groups:

### Autoregressive Features

* `lag_1`, `lag_2`, `lag_3`, `lag_4`
* `rolling_mean_3`, `rolling_std_3`

### Climate Features

* Rainfall anomaly
* Temperature anomaly
* Rain × temperature interaction
* Drought risk proxy

### Trend Features

* Year-centered
* Decade indicator
* Yield momentum

### Target Transformation

* Log-transformed yield for skew normalization

SHAP analysis shows:

* 77% predictive power from lag features
* 18% from climate variables
* 5% from regional effects

---

## Modeling Strategy

Three model types were benchmarked:

| Model                 | RMSE   | MAPE  | Insight          |
| --------------------- | ------ | ----- | ---------------- |
| Naive (lag_1 only)    | 12,233 | 5.12% | Baseline         |
| Climate-only          | 8,542  | 3.48% | Weather impact   |
| Full (lags + climate) | 3,400  | 1.47% | Best performance |

### Algorithms Used

* XGBoost
* LightGBM
* Optuna
* SHAP
* Scikit-learn

Hyperparameter tuning performed using Optuna with time-aware cross-validation.

---

## Validation Strategy

To prevent data leakage:

* Time-based train/test split
* 5-fold TimeSeriesSplit
* Lag features generated with shift
* Rolling statistics using only past values

Cross-validation results were stable:

* CV Mean RMSE: 3,392 ± 11
* Train/Test gap minimal → no overfitting

---

## Uncertainty Quantification

Quantile regression implemented using LightGBM:

* 10th percentile
* 50th percentile (median)
* 90th percentile

Provides 80% confidence intervals for decision support.

Example output:

Prediction: 35,072 hg/ha
Confidence Interval (80%): 34,834 – 39,400 hg/ha

---

## Deployment

Two deployment options:

### Interactive Interface

Built using:

* Gradio

Features:

* Country & crop selection
* Real-time predictions
* Multi-year recursive forecasting
* Interactive visualizations

### REST API

Built using:

* Flask

Endpoints:

* `/predict`
* `/forecast`
* `/health`
* `/metrics`

Model serialized using Joblib.

---

## Recursive Forecasting

Supports multi-year forecasting by feeding predictions back as lag inputs.

Handles:

* Error propagation
* Uncertainty widening
* Trend stability

---

## Model Interpretability

Using SHAP:

Top features:

1. lag_1 (58%)
2. rolling_mean_3 (15%)
3. rain_anomaly (8%)

Key insight:
Yield is strongly autoregressive but climate significantly adjusts outcomes.

---

## Business Value

Stakeholder impact:

* Farmers: better planting decisions
* Governments: food security forecasting
* Traders: supply prediction
* Insurance: risk modeling
* Agribusiness: inventory optimization

For a 1,000-hectare farm:
A 5% planning improvement could yield ~$75,000 annual value.

---

## Tech Stack

* Python
* Pandas, NumPy
* XGBoost, LightGBM
* Optuna
* SHAP
* Scikit-learn
* Gradio
* Flask
* Git

---

## Repository Structure

```
src/
│
├── data_processing/
├── feature_engineering/
├── models/
├── evaluation/
├── deployment/
│
├── train.py
├── predict.py
└── utils.py
```

---

## Key Strengths of This Project

* Strong time-series validation (no leakage)
* Quantified uncertainty (not just point predictions)
* Benchmark comparison across model types
* SHAP-based interpretability
* Production-ready deployment
* Multi-year recursive forecasting

---

## Limitations

* Heavy dependence on autoregressive structure
* No satellite or soil data included
* Recursive forecasting compounds error over long horizons

---

## Future Improvements

* Soil quality integration
* Satellite NDVI indices
* Crop-specific models
* Spatial feature encoding
* Economic factor integration

---

