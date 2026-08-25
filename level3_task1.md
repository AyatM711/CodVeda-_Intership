# Task 1: Predictive Modeling (Classification) — Analysis Summary
**Level:** 3 (Advanced)
**Dataset:** `churn-bigml-80.csv` (2,666 telecom customer records, 20 columns)
**Tools:** Python, pandas, scikit-learn, matplotlib, seaborn

---

## Objective

Build and evaluate classification models to predict whether a telecom customer will churn (cancel their service), using their account and usage data.

## Data Preparation

The dataset had **no missing values** and no duplicate handling was required. Preprocessing steps applied:

| Step | Details |
|---|---|
| Target encoding | `Churn` converted from `True`/`False` to `1`/`0` |
| Binary categorical encoding | `International plan` and `Voice mail plan` mapped from `Yes`/`No` to `1`/`0` |
| Dropped columns | `State` (51 unique values, high cardinality, low individual predictive signal for a tree/linear model at this dataset size) and `Area code` (only 3 values, not meaningfully related to churn) |
| Feature scaling | `StandardScaler` applied for Logistic Regression only — tree-based models (Decision Tree, Random Forest) don't require scaling since they split on raw thresholds |

**Class balance:** The target is imbalanced  **14.6% of customers churned** (388 of 2,666). This was addressed by using a **stratified train/test split**, ensuring both sets preserve the same churn ratio.

**Split:** 80% train (2,132 records) / 20% test (534 records).

## Methodology

Three classification models were trained and compared using default parameters:

1. **Logistic Regression** : a linear baseline model
2. **Decision Tree** : a simple non-linear model, prone to overfitting
3. **Random Forest** : an ensemble of decision trees, generally more robust

Each was evaluated on the held-out test set using **accuracy, precision, recall, and F1-score** — with F1-score used as the primary comparison metric, since accuracy alone can be misleading on an imbalanced dataset like this one (a model that always predicts "no churn" would already score 85% accuracy while being useless).

## Results: Baseline Model Comparison

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---|---|---|---|
| **Random Forest** | 0.959 | 0.983 | 0.731 | **0.838** |
| Decision Tree | 0.904 | 0.680 | 0.654 | 0.667 |
| Logistic Regression | 0.841 | 0.405 | 0.192 | 0.261 |

**Interpretation:**

- **Random Forest was the clear winner**, with the best F1-score by a wide margin. It correctly flagged most churners while keeping false alarms very low (98.3% precision).
- **Logistic Regression struggled significantly with recall (19.2%)** — it missed 4 out of 5 actual churners. This suggests the relationship between the features and churn isn't well captured by a simple linear boundary; there's meaningful non-linearity that tree-based models handle better.
- **Decision Tree performed reasonably** but was outperformed by Random Forest on every metric, consistent with Random Forest's key advantage: averaging many trees reduces overfitting compared to a single tree.

## Hyperparameter Tuning (GridSearchCV)

Random Forest, as the best baseline model, was tuned further using 5-fold cross-validated grid search over:

- `n_estimators`: [100, 200]
- `max_depth`: [None, 10, 20]
- `min_samples_split`: [2, 5]

**Best parameters found:** `{'max_depth': None, 'min_samples_split': 5, 'n_estimators': 100}`
**Best cross-validated F1-score:** 0.8243

### Tuned Model — Test Set Performance

| Metric | Value |
|---|---|
| Accuracy | 0.9494 |
| Precision | 0.9333 |
| Recall | 0.7179 |
| F1-Score | 0.8115 |

**Confusion Matrix (tuned Random Forest):**

| | Predicted: No Churn | Predicted: Churn |
|---|---|---|
| **Actual: No Churn** | 452 | 4 |
| **Actual: Churn** | 22 | 56 |

**Interpretation:** Out of 78 customers who actually churned in the test set, the tuned model correctly caught 56 (71.8% recall) while only misclassifying 4 loyal customers as churn risks (out of 456) — a strong balance of catching real churners without generating excessive false alarms.

## Feature Importance

The tuned Random Forest's feature importances revealed the strongest churn predictors:

1. **Total day charge** — the single strongest predictor
2. **Total day minutes** — closely tied to charge (as expected, since charge is derived from minutes)
3. **Customer service calls** — customers calling support more often are meaningfully more likely to churn
4. **International plan** — subscribers to this plan show a distinctly different churn pattern

Interestingly, **evening, night, and weekend usage metrics contributed far less** to the model's predictions than daytime usage and service call frequency — suggesting churn is driven primarily by daytime cost sensitivity and support experience, not overall usage volume.

## Visual Findings
<img width="875" height="216" alt="image" src="https://github.com/user-attachments/assets/135d5b51-c3d3-437d-b143-f42a8992d745" />
  


- **Left panel:** Bar comparison of all four metrics across the three baseline models — visually confirms Random Forest's consistent lead.
- **Center panel:** Confusion matrix for the tuned Random Forest, showing strong true-negative performance and solid true-positive recall.
- **Right panel:** Feature importance ranking, highlighting daytime usage and customer service calls as the dominant churn signals.

## Conclusion

Random Forest — both in its default and tuned forms — substantially outperformed Logistic Regression and Decision Tree for this churn prediction task, primarily due to its ability to capture non-linear relationships between usage patterns and churn behavior. The tuned model achieves strong precision (93%) with reasonable recall (72%), making it suitable for a retention campaign where the business wants to target likely churners without wasting outreach on many false positives.

**Business takeaway:** Retention efforts should prioritize customers with high daytime charges and 4+ customer service calls these are the two clearest early-warning signals identified by the model.

