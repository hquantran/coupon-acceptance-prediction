# Data-Driven Coupon Acceptance Prediction

### Optimizing Digital Offers for Drivers 🚗🎟️

## 📌 Table of Contents

* [🎯 Project Overview](#-project-overview)
* [📊 Business Context & Key Metrics](#-business-context--key-metrics)
* [💾 Data](#-data)
* [🛠️ Methodology & Feature Engineering](#-methodology--feature-engineering)

  * [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)
  * [Model Selection & Tuning](#model-selection--tuning)
* [📊 Key Findings](#-key-findings)

  * [Feature Importance](#feature-importance)
  * [Top Acceptance Drivers (EDA)](#top-acceptance-drivers-eda)
* [📈 Final Model Performance](#-final-model-performance)
* [💰 Business Recommendations](#-business-recommendations)
* [🚀 Next Steps](#-next-steps)

---

## 🎯 Project Overview

This project develops a machine-learning model to **predict the likelihood that a food delivery or ride-hailing driver will accept a digital coupon offer**.

The goal is to shift from **broad, untargeted coupon blasts** toward **data-driven targeting** that improves conversion rates and reduces wasted marketing spend.

Key components include:

* Exploratory Data Analysis (EDA)
* Feature Engineering
* Model selection (Logistic Regression, Decision Tree, Random Forest)
* Hyperparameter tuning (final optimized Random Forest)

---

## 📊 Business Context & Key Metrics

Duber currently sends a large volume of digital coupons, with many going unused—leading to wasted voucher costs and lost revenue.

### 💸 Cost of Prediction Errors

| Error Type              | Description                                                | Business Impact                            |
| ----------------------- | ---------------------------------------------------------- | ------------------------------------------ |
| **False Negative (FN)** | Predicts a driver will **not** accept, when they **would** | **$20 lost revenue** per missed conversion |
| **False Positive (FP)** | Predicts a driver **will** accept, when they **won’t**     | **$10 wasted voucher** per case            |

### 🎯 Model Optimization Priorities

Given FN costs **2× higher** than FP, the model is optimized for:

1. **High Recall** – minimize missed conversions
2. **Strong F1-Score** – balance recall with precision

---

## 💾 Data

* **Source:** UCI Machine Learning Repository
* **Observations:** 12,684
* **Features:** 26 (driver demographics, trip characteristics, environment)
* **Target:** `Y` = Coupon Accepted (1) / Rejected (0)
* **Baseline acceptance rate:** **43.2%**

---

## 🛠️ Methodology & Feature Engineering

### Data Cleaning & Preprocessing

A scikit-learn pipeline was used for consistent preprocessing:

#### ✔ Handling Missing Data

* Removed `car` column (99% missing)
* Mode imputation for remaining sparse categorical fields

#### ✔ Feature Engineering

* Combined three binary distance features into **one ordinal `distToCoupon`**
* Removed original distance columns

#### ✔ Encoding & Scaling

| Feature Type                                       | Method              |
| -------------------------------------------------- | ------------------- |
| Ordinal categories (e.g., age, income, Bar visits) | **OrdinalEncoder**  |
| Nominal categories (weather, time, coupon types)   | **One-Hot Encoder** |
| Continuous (temperature)                           | **MinMaxScaler**    |

---

## Model Selection & Tuning

### Baseline Model Comparison

| Model               | Test Recall | Test F1   | Train Time (s) | Overfitting?                  |
| ------------------- | ----------- | --------- | -------------- | ----------------------------- |
| Logistic Regression | 0.778       | 0.732     | 0.17           | Low                           |
| Decision Tree       | 0.717       | 0.717     | 0.13           | **Severe** (Train F1 = 0.999) |
| Random Forest       | **0.817**   | **0.779** | 2.03           | Moderate                      |

➡️ **Selected Final Model:** Optimized **Random Forest Classifier**

* Best baseline performance
* Tuned using balanced class weights for Recall / F1 trade-off

---

## 📊 Key Findings

### Feature Importance (Optimized Random Forest)

Top 5 influential features:

1. **CoffeeHouse visit frequency**
2. **Restaurant (<$20) coupon type**
3. **Carry out & Take Away coupon**
4. **Bar visit frequency**
5. **2-hour expiration window**

---

### Top Acceptance Drivers (EDA Insights)

| Factor          | Impact                                          |
| --------------- | ----------------------------------------------- |
| **Coupon Type** | Carry Out & Take Away (~75% acceptance) highest |
| **Time of Day** | Peaks at **2 PM** and **6 PM**                  |
| **Weather**     | Sunny days → significantly higher acceptance    |
| **Distance**    | <15 min = highest acceptance                    |
| **Income**      | $50k–$62.5k bracket most responsive             |

---

## 📈 Final Model Performance

| Metric        | Test Score | Business Meaning                                         |
| ------------- | ---------- | -------------------------------------------------------- |
| **Recall**    | **0.751**  | Captures 75.1% of actual acceptors → fewer $20 FN losses |
| **Precision** | 0.760      | 76% of offers sent are accepted                          |
| **F1-Score**  | 0.755      | Strong balance between conversions & voucher efficiency  |

---

## 💰 Business Recommendations

### 1️⃣ Timing & Weather Optimization

* Send coupons during **10 AM–2 PM** and **6 PM** peaks
* Prefer sunny conditions; reduce during rain/snow

### 2️⃣ Prioritize High-Performing Offer Types

* Focus on **Carry Out & Take Away** & **Restaurant (<$20)** coupons
* Limit **Bar** coupons unless highly confident in acceptance

### 3️⃣ Geo-Targeting & Expiration Strategy

* Send coupons only when driver is within **5–15 minutes** of location
* Use **2-hour expiration** to boost urgency

### 4️⃣ Demographic Targeting

* Prioritize income group **$50k–$62.5k**

### 5️⃣ Automated Model Integration

Use a probability threshold (e.g., **p > 0.7**) to balance cost:

* Reduce False Negatives ($20 loss)
* Reduce False Positives ($10 waste)

---

## 🚀 Next Steps

* **A/B Test Pilot Deployment**
  Compare model-driven targeting vs. historical untargeted blasts

* **Build a Feedback Loop**
  Retrain with real redemption data to prevent model drift

* **Feature Expansion**
  Incorporate live passenger type, real-time route patterns, etc.
