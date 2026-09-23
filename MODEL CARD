# Model Card: CREDIT CARD FRAUD DETECTION

## 1. Model Details
* **Developer:** Elisavet Batagianni
* **Date:** September 2026
* **Version:** v1.0 (Random Forest + SMOTE
* **Type:** binary classification (Random Forest Classifier)
* **Frameworks:** scikit-learn, imbalanced-learn (SMOTE), MLflow, SHAP, Evidently AI

## 2. Intended Use
* **Primary intended use:** This model is developed in order to detect credit card frauds.
* **Users that may be interested:** risk management and fraud operation teams in banks and money institutes that value transaction safety and risk-free money exchanges.

## 3. Training & Evaluation Data
* **Dataset:** two days of September 2013 credit card transactions – European Credit Card Fraud Dataset (Kaggle) -> 284,807 transactions, 284,315 legal, 492 fraud, 31 features of interest
* **Imbalanced ratio:** 0.17% fraud
* **Preprocessing:**
  1. Robust scaling on the ‘Time’ and ‘Amount’ features
  2. Stratified 80/20 train-test split
  3. SMOTE (Synthetic Minority Over-sapling Technique) only on the training set (X_train) for the classes balancing on 50/50
  4. Final model verdict: Random Forest (SMOTE) is the best model of production, while it succeeded on the best balance between high detection of correct frauds (Recall = 84%) and low percentage of false alerts on legal transactions (Precision = 85%)

| Model | Precision | Recall | F1-score | AUPRC | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Logistic regression SMOTE | 0.0580 | 0.9184 | 0.1092 | 0.0292 | 0.4313 |
| Random Forest SMOTE | 0.8454 | 0.8367 | 0.8410 | 0.0292 | 0.4313 |
| XGBoost SMOTE | 0.6929 | 0.8571 | 0.7602 | 0.0292 | 0.4313 |
| LightGMB SMOTE | 0.4913 | 0.8673 | 0.6273 | 0.0292 | 0.4313 |

## 5. Model Explainability (SHAP analysis)
Top 5 most important features after analysis for fraud detection:
`V14` > `V12` > `V10` > `V17` > `V4`

## 4. Failure Modes
* **False positives (a user gets blocked; a normal transaction is evaluated as illegal):** 15% of the alerts are false, as legal transactions are falsely considered illegal. There is a need of a mechanism that uses smooth and gentle ways of user notification (SMS, email). Blocking their cards may lead to dissatisfaction => customer friction due to unnecessary card declines, operational cost due to call center overload on card verification
* **False negatives (a fraud is not detected; the transaction is fake but the model classifies it as legal):** 16% of frauds are not spotted. The model needs further training in more data, through more sophisticated algorithms. Drift must be closely monitored as its potential growth is the main metric that makes the retraining of the model absolutely crucial => direct financial loss, bank reputation and trust under attack, individual and bank security problem, bank chargebacks

## 5. MLOps Infrastructure & Considerations
* **Experiment Tracking:** complete log of all the parameters, metrics, diagrams through MLflow.
* **Data Drift Detection:** Evidently AI is used for continuous monitoring of the data drift between Training and Production data sets.

## 6. Ethical Considerations
Fraud detection is classified as a high-risk AI system under the EU AI Act:

Due to modern EE legislation and policy, our model needs to be constrained in 5 basic lines:

1. **Transparency & Explainability:**
   * **Need:** The transactions need to be labeled in a way that can be justified and reassessed without exposure of personal data
   * **Offer:** SHAP values (Vi) give a clear picture over the data importance towards the regulators and the customers
2. **Data Governance & Bias Control:**
   * **Need:** Data must remain true, unbiased and checked through techniques of alteration
   * **Offer:** SMOTE is applied only on the training data (X_train), and in combination to PCA transformation it is sure that no personal sensitive data are used or exposed.
3. **Technical Robustness & Monitoring:**
   * **Need:** System must be resilient to changes and errors throughout its life-circle
   * **Offer:** Evidently AI makes real-time data drift detection possible, MLflow fully responsible for versioning and traceability of the model
4. **Human Oversight:**
   * **Need:** system of high danger must not be completely automated due to need of human action in crucial decisions
   * **Offer:** the threshold settled makes sure that cases of important exchanges which are labeled as ‘critical’ will be reassessed by a human agent
5. **Documentation & Model Cards:**
   * **Need:** keep of a detailed report before market
   * **Offer:** Model Card creation offers a fully detailed log with transparency, detailed metrics, limitations and failure modes.
