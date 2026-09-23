# Credit Card Fraud Detection

# Project Overview

Based on the critical need for transaction security, this project uses Machine Learning aiming to recognize card activity that corresponds to fraud users in real time. The primary goal is to allow financial institutions and individual users to detect fraud transactions promptly, preventing financial losses and further charges.

### Approach
* **AI-driven detection:** An end-to-end production ready pipeline that identifies spending patterns and flags transactions of high risk.
* **Algorithm benchmarking:** Evaluated four algorithms (*Random Forest*, *Logistic Regression*, *LightGBM*, *XGBoost*) on both imbalanced raw data and post-SMOTE balanced data.
* **Optimal selection:** Identified the most cost-effective and operational model that balances high fraud recall with low false positives.

---

## Key Metrics & Analysis

### 1st Metric: Exploratory Data Analysis & Dataset Integrity
* **Dataset size:** 284,807 transactions with 31 features.
* **Feature Breakdown:**
  * `V1` – `V28`: PCA-transformed continuous variables (anonymized for privacy).
  * `Time`: Elapsed seconds between the current transaction and the first transaction in the dataset.
  * `Amount`: Transaction monetary value.
  * `Class` (Target): Binary label (`0` = Legal, `1` = Fraud).
* **Data types:** `float64` for numerical feature precision, `int64` for the binary target variable.
* **Data quality:** 0 Missing Values (0.0% Nulls) across all rows.

> **Takeaway:** The dataset is fully clean and structured, making it immediately ready for feature scaling, train-test splitting, and SMOTE resampling.

---

### 2nd Metric: Ensuring Data Quality & Fairness
* **Train / Test split:** 
  * `X_train` shape: `(227845, 30)` $\rightarrow$ **80%** of the data used for model training
  * `X_test` shape: `(56962, 30)` $\rightarrow$ **20%** of the data used for final model evaluation 
* **Stratification Check:**
  * Train set fraud ratio: **0.173%** (394 fraud cases)
  * Test set fraud ratio: **0.172%** (98 fraud cases)

> **Takeaway:** Stratified sampling guarantees that both training and evaluation subsets preserve the original class imbalance (0.17%), preventing sampling bias and ensuring a realistic model approach.

---

### 3rd Metric: Baseline Model Evaluation (Imbalanced Data)

| Model | Precision | Recall | F1-score | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: |
| **Random Forest** | 0.9412 | 0.8163 | 0.8743 | 0.9630 |
| **XGBoost** | 0.8966 | 0.7959 | 0.8432 | 0.9219 |
| **Logistic Regression** | 0.8289 | 0.6429 | 0.7241 | 0.9559 |
| **LightGBM** | 0.1183 | 0.2041 | 0.1498 | 0.4313 |

#### Metric Definitions:
* **Precision:** Which of the trades marked as fraud are actually fraud.
* **Recall:** Which percentage of the frauds in the test set were spotted.
* **F1-score:** Precision’s and recall’s harmonic mean (model’s overall quality).
* **AUC-ROC:** Whole model ability to distinguish true from false transactions through different probability thresholds.

> **Takeaway:** Random Forest achieved the highest baseline performance ($F1 = 0.8743$). Recall at 81.63% means that 18.4% of frauds were not detected, which highlights the need for class balancing via SMOTE.

---

### 4th Metric: SMOTE Model Evaluation & Trade-off Analysis

| Model | Precision | Recall | F1-score | AUPRC | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Random Forest + SMOTE** | **0.8454** | **0.8367** | **0.8410** | 0.0292 | 0.4313 |
| **XGBoost + SMOTE** | 0.6929 | 0.8571 | 0.7602 | 0.0292 | 0.4313 |
| **LightGBM + SMOTE** | 0.4913 | 0.8673 | 0.6273 | 0.0292 | 0.4313 |
| **Logistic Regression + SMOTE** | 0.0580 | 0.9184 | 0.1092 | 0.0292 | 0.4313 |

> **Takeaway:** While linear models achieve high Recall after SMOTE, Precision collapses to almost 5.8%, making them unusable in production. **Random Forest + SMOTE** maintains the highest F1-score, balancing high fraud detection with minimal false alarms.

---

### 5th Metric: Pre vs. Post-SMOTE Performance Visual Trade-Off
The comparison charts illustrate the impact of SMOTE resampling for all evaluated algorithms:
* **Recall Impact (Left):** All models show significant recall gains post-SMOTE, notably Logistic Regression ($0.64 \rightarrow 0.92$) and LightGBM ($0.20 \rightarrow 0.87$).
* **Precision Trade-Off (Right):** Linear models show a drop in Precision (Logistic Regression dropped to $0.06$), leading to acceptable recall at the cost of many false alarms.
* **Optimal Balance:** Random Forest maintained the greatest resilience, with **0.85 Precision** and **0.84 Recall**.

![pre vs. post SMOTE analysis](pre_vs._post_SMOTE_graph.png)

> **Takeaway:** Visual analytics confirm that **Random Forest + SMOTE** achieves the highest area under the Precision-Recall spectrum, minimizing operational noise while securing critical fraud detection coverage.

---

### 6th Metric: Model Explainability & Features of Importance (SHAP Analysis)
* **Top 5 Predictors Ranking:** `V14` > `V12` > `V4` > `V3` > `V10`
* **Behavioral Insights:**
  * **Direct relationship:** `V4` $\rightarrow$ High feature values directly push the prediction toward a fraud classification.
  * **Inverse relationship:** `V14`, `V12`, `V3`, `V10` $\rightarrow$ Very low feature values correlate strongly with fraud detection behavior.

> **Takeaway:** SHAP analysis provides explanations for the variables that flag each transaction, making it easier for fraud analyst teams to address high-risk alerts.

---

## Key Findings

* **Optimal Model Performance:** Among all evaluated algorithms, **Random Forest + SMOTE** emerged as the winning production model, achieving an exceptional balance of **84% Recall** (detecting 5 out of 6 frauds) and **85% Precision** (minimizing false alarms).
* **Mitigation of Class Imbalance:** Applying SMOTE exclusively to the training set successfully resolved the 0.17% fraud sparsity without causing data leakage into evaluation benchmarks.
* **Explainable AI (XAI):** TreeSHAP analysis identified `V14`, `V12`, `V4`, `V3`, and `V10` as the primary indicators of fraudulent activity, providing clear interpretability for banking audit protocols.
* **Production & Governance Readiness:**
  1. Integrated **Evidently AI** for real-time Data Drift monitoring to trigger automated retraining.
  2. Logged all artifacts and metrics via **MLflow**.
  3. Fully compliant with **EU AI Act (Article 13 & 14)** requirements for High-Risk Financial AI Systems through explainability and Human-in-the-Loop decision thresholds.
