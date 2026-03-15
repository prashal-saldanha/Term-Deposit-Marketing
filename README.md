# 📈 Term Deposit Marketing – Customer Subscription Prediction

## 📌 Project Overview
This project was developed for a fast-growing machine learning startup specializing in AI-driven decision systems for the European banking sector.

The company partners with financial institutions to:

- Improve customer engagement
- Increase marketing conversion rates
- Enable data-driven decision making under strict regulatory and interpretability requirements

In highly competitive retail banking environments, direct marketing campaigns via call centers remain a key revenue channel. However, these campaigns are often:

- Expensive to operate
- Intrusive for customers
- Inefficient due to poor targeting

This project builds a machine learning system that predicts whether a customer will subscribe to a term deposit after a marketing call. Beyond prediction, it focuses on understanding which customer segments should be prioritized, enabling banks to move from mass outbound calling to intelligent targeted marketing.

---

## 🧠 Business Problem
Banks conduct large-scale phone campaigns to promote term deposit investment products. However, most contacted customers do not subscribe, resulting in:

- Wasted operational resources
- Poor customer experience
- Low marketing ROI

This project addresses the problem by answering key business questions:

- Which customers are most likely to subscribe?
- Which features influence purchasing decisions?
- How can marketing campaigns be optimized?

**Goal:** Enable smarter customer targeting and improved campaign efficiency.

---

## 📊 Data Description
The dataset originates from direct marketing campaigns conducted by a European bank.  

- **Records:** 40,000 customers  
- **Features:** 13  
- **Target:** 1  
- **Total Variables:** 14  

The data contains a mixture of demographic information, financial indicators, and marketing interaction details. All personally identifiable information (PII) has been removed.

### 🧾 Feature Description

**Demographic Information**

| Feature   | Description           |
|----------|----------------------|
| age      | Age of the customer   |
| job      | Occupation type       |
| marital  | Marital status        |
| education| Education level       |

**Financial Information**

| Feature  | Description                  |
|----------|------------------------------|
| default  | Has credit in default         |
| balance  | Average yearly account balance (€) |
| housing  | Has housing loan             |
| loan     | Has personal loan            |

**Campaign & Contact Information**

| Feature   | Description                     |
|-----------|---------------------------------|
| contact   | Communication method            |
| day       | Last contact day of the month   |
| month     | Last contact month              |
| duration  | Duration of last call (seconds) |
| campaign  | Number of contacts during campaign |

**Target Variable**

| Variable | Meaning                       |
|----------|-------------------------------|
| y = 1    | Customer subscribed to term deposit |
| y = 0    | Customer did not subscribe          |

---

## 🎯 Project Goals

**Primary Goal:**  
Build a binary classification model that predicts whether a customer will subscribe to a term deposit.

**Secondary Goals:**  

- Identify high-value customer segments  
- Understand drivers of subscription decisions  
- Ensure model interpretability for business stakeholders  
- Provide actionable insights for marketing teams  

---

## 📏 Success Metrics

**Primary Metric:**  
- Classification Accuracy (Target benchmark: ≥ 81%)  

**Additional Metrics:**  
- ROC–AUC (important due to class imbalance)  
- Recall  
- Model stability across cross-validation folds  

### ⚠️ Key Data Challenge – Class Imbalance

| Outcome       | Proportion |
|---------------|------------|
| No subscription | 92.8%     |
| Subscription    | 7.2%      |

**Implications:** Accuracy alone is misleading; models must be evaluated using ROC-AUC and recall for the minority class.

---

## 🔍 Project Workflow

### 1️⃣ Data Cleaning
- Verified no missing values  
- Replaced "unknown" categorical values with mode  
- Converted target variable: `yes → 1`, `no → 0`  

### 2️⃣ Exploratory Data Analysis (EDA)
- **Campaign Timing:** Massive spike in calls during May  
- **Customer Demographics:** Major groups – Blue-collar, Management, Technicians, Married individuals  
- **Financial Patterns:** Successful subscribers typically have higher account balances and fewer loans  

### 3️⃣ Data Preparation
1. **Outlier Handling:** IQR-based Winsorization to cap extreme values
2. **Feature Encoding:**
   - **Numerical Features:** MinMaxScaler
   - **Categorical Features:** Binary Encoding (to reduce dimensionality)

---

## 🤖 Models Evaluated

| Model                   | ROC-AUC (approx) |
|-------------------------|----------------|
| Decision Tree           | 0.90           |
| Gaussian Naive Bayes    | 0.88           |
| Random Forest           | 0.93           |
| K-Nearest Neighbors     | 0.76           |
| Support Vector Machine  | 0.92           |
| Logistic Regression     | 0.91           |
| Balanced Random Forest  | 0.93           |
| XGBoost                 | 0.92           |
| RUSBoost                | 0.60           |

**Shortlisted Models:** Random Forest, Balanced Random Forest, XGBoost  

**Cross Validation (5-Fold Stratified):**

| Model                  | ROC-AUC |
|------------------------|---------|
| Random Forest          | 0.929   |
| Balanced Random Forest | 0.929   |
| XGBoost                | 0.920   |

**Further Shortlisted Models:** Random Forest, Balanced Random Forest  

---

## ⚙️ Hyperparameter Optimization

**RandomizedSearchCV** (5-fold, 30 iterations, metric: ROC-AUC)  

**Best Random Forest Parameters:**
```
n_estimators = 500
max_depth = 30
min_samples_split = 2
min_samples_leaf = 4
max_features = sqrt
```
**Best Random Forest CV Score:** 0.9335281661519167

**Best Balanced Random Forest Parameters:**
```
n_estimators = 200
max_depth = None
min_samples_split = 2
min_samples_leaf = 1
max_features = sqrt
```
**Best Balanced Random Forest CV Score:** 0.9280863135376487

**Final Selected Model:** Random Forest

---

## 🔎 Feature Importance for Random Forest

Top predictors identified by Random Forest:  
- Call duration  
- Contact day  
- Account balance  
- Customer age  
- Month of contact  
- Campaign frequency  
- Housing loan status
  
---

## 🏆 Final Model Performance

A reduced feature model using the top 10 features achieved slightly improved performance.

| Metric      | Score |
|------------|-------|
| Accuracy    | 91.8% |
| ROC-AUC     | 0.937 |
| Recall      | 0.74  |

---

## 👥 Customer Segmentation (Clustering)

**Method:** K-Means clustering on successful subscribers only  
**Optimal clusters:** 4 (determined via Elbow method & Silhouette analysis)

**Customer Segment Insights:**

| Cluster | Characteristics | Priority |
|---------|----------------|----------|
| 3 – High-Value Investors | Highest balances, management professionals, tertiary education, no housing loans, low personal loans | Top Priority |
| 2 – Educated Strivers | Highly educated, strong financial potential, currently with housing loans | Secondary Priority |

---

## 💡 Key Drivers of Customer Subscription

1️⃣ **Engagement (Call Duration):** Longer calls (>3 minutes) increase subscription probability.  
2️⃣ **Ability to Invest (Financial Capacity):** Higher balances, no loans → higher likelihood.  
3️⃣ **Opportunity Timing (Month of Contact):** Best months – April & August.  

**Recommendations:**  
- Train agents for longer conversations  
- Prioritize financially capable customers  
- Focus campaigns during high-conversion months  

---

## 📈 Operational Recommendations

| Factor           | Business Action |
|-----------------|----------------|
| Call Duration     | Train agents to extend conversations |
| Customer Profile  | Prioritize management & tertiary education |
| Loan Status       | Focus on customers without loans |
| Campaign Frequency| Avoid excessive repeated contacts |
| Timing            | Focus campaigns in April and August |

---

## 🚀 Business Impact

- Increase campaign conversion rates  
- Reduce unnecessary outbound calls  
- Improve customer experience  
- Focus marketing budgets on high-value leads  
- Shift from mass calling → intelligent targeting  

---

## 🏁 Final Thoughts

This project demonstrates the full lifecycle of an applied machine learning solution:

- Data cleaning and exploratory analysis  
- Handling severe class imbalance  
- Model comparison and hyperparameter tuning  
- Feature importance analysis  
- Customer segmentation  
- Translating model insights into actionable business strategy  

The focus is not only on prediction accuracy but also on interpretability, strategic insights, and real-world usability for banking operations.
```
