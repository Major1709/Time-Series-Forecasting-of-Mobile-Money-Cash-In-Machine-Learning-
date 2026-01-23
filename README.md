# 📊 Time Series Forecasting of Mobile Money Cash-In (Machine Learning)

## 🧠 Project Overview

This project focuses on **forecasting the monthly future Cash-In value of Mobile Money agents** using historical data and **time series Machine Learning techniques**.

The goal is to support **liquidity management**, **operational planning**, and **data-driven decision making** by anticipating future transaction values.

---

## 🎯 Objective

> **Predict the total Cash-In value for month t+1**
> using all information available up to month t.

---

## 🗂️ Dataset

* Granularity: **Monthly**
* Time period: **2007 – 2023**
* Final dataset size after feature engineering: **85 observations**

### Main variables

* `Year`
* `Mouth`
* `Active_Agents`
* `Total_Registered_Mobile_Money_Accounts_Millions`
* `Total_Agent_Cash_in_Cash_Out_Volume_millions`
* `Total_Agent_Cash_in_Cash_Out_Value_KSh_billions` (target variable)

---

## 🔧 Methodology

### 1️⃣ Data Preparation

* Creation of a proper `date` variable (`YYYY-MM-01`)
* Strict chronological sorting
* Cyclical encoding of month:

  * `month_sin`, `month_cos`
* Logarithmic transformation of the target (`log1p`) to stabilize variance

---

### 2️⃣ Feature Engineering (Core of the project)

* Creation of **time lags**:

  * `lag1` (previous month)
  * `lag3` (previous quarter)
  * `lag12` (same month last year)
* Addition of business-related explanatory variables
* Careful prevention of **temporal data leakage**

---

### 3️⃣ Target Definition

```
Y(t+1) = Cash-In value of the next month
```

The last observation is removed since the future value is unknown.

---

### 4️⃣ Temporal Validation Strategy

* **Chronological train/test split**:

  * 80% training (earlier data)
  * 20% testing (most recent data)
* No shuffling, no standard K-Fold cross-validation

---

## 🤖 Models Evaluated

### 🔹 Naive Baseline

> Predict that the next month equals the previous month (`lag1`)

### 🔹 Random Forest Regressor (Final Model)

* Robust on small datasets
* Strong performance on autoregressive signals
* Good bias–variance trade-off

### 🔹 XGBoost Regressor

* Tested for comparison
* Slightly underperformed Random Forest on this dataset

---

## 📈 Results (Original Scale)

| Model             | MAE      | RMSE     |
| ----------------- | -------- | -------- |
| Naive baseline    | ~587     | ~592     |
| **Random Forest** | **~262** | **~270** |
| XGBoost           | ~276     | ~284     |

👉 **Random Forest** significantly outperforms the naive baseline and is selected as the final model.

---

## 🔍 Model Interpretability

* SHAP values were used for explainability
* Most important features:

  1. `lag1` (short-term inertia)
  2. `lag12` (annual seasonality)
  3. `Total_Agent_Cash_in_Cash_Out_Volume_millions`
* Feature effects are **economically and temporally coherent**

---

## 📉 Visual Evaluation

* Predictions closely follow the overall trend
* Systematic underestimation of extreme peaks (expected behavior for Random Forest)
* Residuals are centered around zero with no visible temporal drift

---

## ⚠️ Limitations

* Relatively small dataset
* Difficulty capturing extreme peaks
* Monthly aggregation limits short-term dynamics

---

## 🚀 Future Improvements

* Forecasting variations (ΔY) instead of levels
* Adding rolling statistics (rolling mean, rolling std)
* Quantile regression to better capture extreme values
* Rolling window backtesting

---

## 🧾 Conclusion

This project demonstrates that a **well-designed time series Machine Learning pipeline**, driven by feature engineering and proper temporal validation, can **significantly outperform a naive baseline** while remaining **robust, interpretable, and production-ready**.

---

## 🛠️ Technologies Used

* Python
* Pandas / NumPy
* Scikit-learn
* XGBoost
* SHAP
* Matplotlib / Seaborn

---

📌 **Author**: *Your Name*
📌 **Role**: Data Scientist
📌 **Project type**: Time Series Machine Learning Portfolio Project
