# 📈 Walmart Store #1 Weekly Sales Forecasting

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)  
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)

![image](https://github.com/user-attachments/assets/f1900499-e6f3-4434-805e-cc6af82b781c)

## Table of Contents

- [Project Overview](#project-overview)  
- [Data & Sample](#data--sample)  
- [Installation](#installation)  
- [Feature Engineering](#feature-engineering)  
- [Modeling](#modeling)  
- [Results](#results)

---

## Project Overview

This repository demonstrates an **end-to-end feature engineering pipeline** and **XGBoost modeling** workflow focused on forecasting weekly sales for **Store #1** only.  

**All work including previously done feature engineering in this project was performed by _Tez1s_.**

It includes:

- Loading pre-processed data for Store #1  
- A suite of engineered features capturing seasonality, holidays, rolling trends, and interactions  
- Training an XGBoost regressor with time‑aware cross‑validation  
- Evaluation of model performance in terms of RMSE and MAE  

> **Note:** All feature engineering was performed by me prior to this project; here you see only the downstream pipeline (features → model).

---

## Data & Sample

Data used in this repo is already cleaned and filtered to **Store #1**. After feature engineering, each row represents one week’s record:

| Date       | Weekly_Sales | Holiday_Flag | Temperature | Fuel_Price |    CPI     | Unemployment | Month | is_holiday | pre_holiday | post_holiday | Weekly_Sales_MA30 | holiday_sales_impact | temp_fuel_interaction | Day | Lag1_Day |
|------------|-------------:|-------------:|-----------:|-----------:|-----------:|-------------:|------:|-----------:|------------:|-------------:|-----------------:|---------------------:|-----------------------:|----:|---------:|
| 2010-02-26 |  1,409,727.59|            0 |       46.63|       2.561| 211.319643 |        8.106 |     2 |          0 |           0 |            0 |       1,576,836.00|                  0.0 |               119.4194 |  26 |      NaN |
| 2010-03-05 |  1,554,806.68|            0 |       46.50|       2.625| 211.350143 |        8.106 |     3 |          0 |           0 |            0 |       1,554,615.00|                  0.0 |               122.0625 |   5 |     26.0 |
| 2010-03-12 |  1,439,541.59|            0 |       57.79|       2.667| 211.380643 |        8.106 |     3 |          0 |           0 |            0 |       1,504,011.00|                  0.0 |               154.1259 |  12 |      5.0 |
| 2010-03-19 |  1,472,515.79|            0 |       54.58|       2.720| 211.215635 |        8.106 |     3 |          0 |           0 |            0 |       1,469,148.00|                  0.0 |               148.4576 |  19 |     12.0 |
| 2010-03-26 |  1,404,429.92|            0 |       51.45|       2.732| 211.018042 |        8.106 |     3 |          0 |           0 |            0 |       1,467,823.00|                  0.0 |               140.5614 |  26 |     19.0 |

---

## Installation

1. **Clone the repository**  
   ```bash
   git clone https://github.com/Tez1s/walmart_xgb_cv.git
   cd walmart_xgb_cv

## Feature Engineering

**All features have been pre‑computed and bundled into `final_walm.csv`:**

- **Rolling Mean (30‑day)**  
  - `Weekly_Sales_MA30`: 30‑day moving average of `Weekly_Sales`.
  
- **Holiday Effects**  
  - `is_holiday`: 1 if the week contains a holiday, else 0  
  - `pre_holiday` / `post_holiday`: Flags for the weeks immediately before and after a holiday  
  - `holiday_sales_impact`: Difference between actual sales and the rolling mean during holiday weeks
  
- **Interaction Terms**  
  - `temp_fuel_interaction`: Product of `Temperature` and `Fuel_Price` to capture joint effects
  
- **Lag Features**  
  - `Lag1_Day`: Sales from the previous week (weekly lag)
  
- **Date Components & Economic Indicators**  
  - `Month`  
  - `Day`  
  - `Holiday_Flag` (original Kaggle flag)  
  - `Temperature`, `Fuel_Price`, `CPI`, `Unemployment`  

---

## Modeling

### Algorithm **XGBoost Regressor** (`xgboost.XGBRegressor`)
- base_score=0.5
- booster='gbtree'
- n_estimators=1000
- early_stopping_rounds=50
- max_depth=3
- learning_rate=0.01

### Cross‑Validation  
- **TimeSeriesSplit** with `n_splits=5`, ensuring each validation fold only trains on past data
- **Each fold trains the model on an increasing subset of data, maintaining temporal order**  

### Evaluation Metrics  
- **RMSE** (Root Mean Squared Error)  
- **MAE** (Mean Absolute Error)

---

## Results

**Model Performance on Store #1 Weekly Sales Forecasting:**

- **📉 RMSE**: `19,274.35`  
- **📈 MAPE**: `1.13%`

These results indicate strong predictive performance, with the model capturing weekly sales trends with high accuracy and minimal percentage error.

## Show Your Support

If you found this project helpful or interesting, please consider giving it a ⭐️!  
Your support encourages further development and helps others discover this work.
