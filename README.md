# 📊 Bank Marketing Subscription Prediction

![Python](https://img.shields.io/badge/Python-3.12-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-orange)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-yellow)
![Status](https://img.shields.io/badge/Project-Completed-success)

---

## 📌 Overview

This project aims to predict whether a client will subscribe to a **term deposit** using data from direct marketing campaigns conducted by a banking institution.

The full machine learning workflow was implemented, including:

- Data cleaning and preprocessing  
- Exploratory Data Analysis (EDA)  
- Feature engineering  
- Model building (Logistic Regression & Random Forest)  
- Hyperparameter tuning  
- Threshold optimization for imbalanced classification  

---

## 🎯 Problem Statement

Given client and campaign-related features, predict:

> Will the client subscribe to a term deposit? (`y = yes / no`)

This is a **binary classification problem** with a **highly imbalanced target variable**.

---

## 📂 Dataset Summary

- Total records: **45,211**
- Features: **17**
- Target: `y`

### 🔹 Target Distribution

- **No**: ~88.3%  
- **Yes**: ~11.7%  

➡️ This strong imbalance heavily impacts model behavior.

---

## 🧹 Data Cleaning & Preprocessing

### 🔻 Dropped Columns

| Column | Reason |
|------|--------|
| `Unnamed: 0` | Index column (no value) |
| `poutcome` | Too many missing values |
| `duration` | Data leakage (not available before prediction) |
| `default` | Quasi-constant (~98% = "no") |

---

### 🔻 Feature Types

#### Numerical Features
- `age`
- `balance`
- `day_of_week`
- `campaign`
- `pdays`
- `previous`

#### Categorical Features (Nominal)
- `job`
- `marital`
- `housing`
- `loan`
- `contact`
- `month`

#### Ordinal Feature
- `education`  
  (encoded as: primary < secondary < tertiary)

---

### 🔻 Missing Values Handling

- Categorical → filled with `"MISSING"`
- Numerical → imputed using median

---

### 🔻 Encoding & Scaling

- One-Hot Encoding → categorical features  
- Ordinal Encoding → education  
- StandardScaler → numerical features  

---

## 🔍 Exploratory Data Analysis (EDA)

### 📊 Key Observations

- Dataset is **highly structured**, not random
- Strong class imbalance toward **"no"**
- Some features strongly influence predictions

---

### 📌 Important Insights

- 👨‍🎓 **Students** have the highest subscription rate  
- 💍 **Singles** subscribe more than married clients  
- 🏠 Clients **without housing loans** are more likely to subscribe  
- 📅 Certain months (**Oct, Mar, Sep, Dec**) show higher success  
- 💰 Higher **balance → higher probability of subscription**  
- 📞 More campaign calls → **lower success rate**

---

### 📉 Numeric Behavior

- `balance` → highly skewed with long tail  
- `campaign` → many outliers  
- `pdays` → dominated by `-1` (never contacted)  
- `previous` → mostly 0  

---

## 🤖 Modeling

### Models Used

1. Logistic Regression  
2. Random Forest Classifier  

A full preprocessing + modeling pipeline was used.

---

## 📈 Logistic Regression Results

### Before Tuning

- High accuracy (~89%)  
- Very poor recall for **"yes"**  

➡️ Model biased toward majority class

---

### After Tuning

Best parameters included:
- `class_weight = balanced`

Results:
- Recall improved significantly  
- Precision still low for minority class  

---

## 🌲 Random Forest Results

### Default Model

- Training accuracy: **100%** (overfitting)
- Poor generalization for "yes"

---

### After Hyperparameter Tuning

Best parameters:

```python
{
  'n_estimators': 100,
  'max_depth': 10,
  'min_samples_split': 5,
  'min_samples_leaf': 2,
  'class_weight': 'balanced'
}
```

### 📊 Performance (Test Set)

- **Accuracy:** 0.84  
- **Recall (yes):** 0.54  
- **Precision (yes):** 0.37  

➡️ Better generalization and improved minority detection  

---

### 🎯 Threshold Optimization

Since the dataset is imbalanced, adjusting the classification threshold was critical.

#### 🔍 Tested Thresholds

| Threshold | Precision | Recall | F1 |
|----------|----------|--------|----|
| 0.5 | 0.37 | 0.54 | 0.43 |
| 0.4 | 0.23 | 0.72 | 0.35 |
| 0.3 | 0.15 | 0.92 | 0.25 |

---

#### ✅ Best Threshold: **0.4**

#### 📈 Final Performance

- **Accuracy:** 0.67  
- **Recall (yes):** 0.72  
- **Precision (yes):** 0.23  
- **F1-score (yes):** 0.35  

---

### 📊 Confusion Matrix

```pgsql
[[6632 3318]
[ 376 977]]
```


---

### 🧠 Final Insights

- Accuracy is misleading due to class imbalance  
- Recall is the most important metric in this problem  
- Threshold tuning significantly improved model usefulness  
- Random Forest captures feature interactions better than Logistic Regression  
- Model successfully identifies majority of potential subscribers  

---

### ⚙️ Tech Stack

- Python  
- Pandas  
- NumPy  
- Scikit-learn  
- Matplotlib  
- Seaborn  

---

## 📌 Conclusion

A complete machine learning pipeline was built from raw data to a tuned model.

The final model achieves a **strong recall (72%)** for the minority class using threshold optimization, making it suitable for identifying potential customers in marketing campaigns.
