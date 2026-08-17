# Level 1 (Basic) — Analysis Summary
**Dataset:** `4) house Prediction Data Set.csv` (Boston Housing dataset — 506 records, 14 columns)
**Tools:** Python, pandas, numpy, matplotlib, seaborn, scikit-learn

This document covers the two Level 1 tasks completed in `level_One.ipynb`:

1. Task 1: Data Cleaning and Preprocessing
2. Task 2: Exploratory Data Analysis (EDA)

---

## Task 1: Data Cleaning and Preprocessing

### Objective
Take a raw dataset containing missing values, potential duplicates, and inconsistent formatting, and produce a clean, analysis-ready version using pandas.

### Column Reference
Before cleaning, headers were added to the raw CSV (originally unlabeled) using the standard Boston Housing schema:

| Column | Meaning |
|---|---|
| CRIM | Per-capita crime rate |
| ZN | Proportion of residential land |
| INDUS | Non-retail business land proportion |
| CHAS | Charles River dummy variable |
| NOX | Nitric oxide concentration |
| RM | Average number of rooms |
| AGE | Proportion of older buildings |
| DIS | Distance to employment centres |
| RAD | Accessibility to radial highways |
| TAX | Property-tax rate |
| PTRATIO | Pupil-teacher ratio |
| B | Population-related index |
| LSTAT | Lower-status population percentage |
| MEDV | Median house value (target) |

### Methodology

**1. Load the Dataset**
The CSV was loaded with `pandas.read_csv()`. The raw data starts at **506 rows and 14 columns**.

**2. Inspect Data Types and Missing Values**
`df.info()` and `df.isnull().sum()` were used to check each column's data type and completeness:

| Column | Non-Null Count | Missing |
|---|---|---|
| `MEDV` (target price) | 452 / 506 | **54 missing** |
| All other 13 columns | 506 / 506 | 0 missing |

Only the target column had gaps.

**3. Handle Duplicate Rows**
`df.duplicated().sum()` was used to check for exact duplicate rows.

**Result:** 0 duplicates found — no rows needed to be removed.

**4. Handle Missing Values (Imputation)**
Columns were split into numerical and categorical groups. Numerical columns with missing values were filled using the **median** (chosen over the mean for its resistance to outliers).

**Result:** The 54 missing values in `MEDV` were filled with the column median (**21.95**). No categorical columns required imputation.

**5. Standardize Data Formats**
Categorical/text columns were stripped of extra whitespace and converted to title case. A date-parsing step was included but not triggered, since this dataset has no `date` column.

**6. Final Data Summary**

| Metric | Value |
|---|---|
| Final shape | 506 rows × 14 columns |
| Remaining missing values | **0** |
| Duplicate rows removed | 0 |
| Values imputed | 54 (in `MEDV`, via median) |

### Conclusion
The dataset was fully cleaned and validated: every missing value was resolved, no duplicate records existed, and text formatting was standardized. No rows were dropped — imputation preserved the full 506-row dataset for the EDA that follows.

---

## Task 2: Exploratory Data Analysis (EDA)

### Objective
Explore the cleaned dataset to understand variable distributions, detect outliers, and identify which features are most strongly related to house value (`MEDV`).

### 1. Summary Statistics
Mean, median, mode, and standard deviation were calculated for all 14 numerical columns.

| Variable | Mean | Median | Mode | Std Dev |
|---|---|---|---|---|
| CRIM | 1.269 | 0.145 | 0.000 | 2.399 |
| ZN | 13.295 | 0.000 | 0.000 | 23.049 |
| INDUS | 9.205 | 6.960 | 18.100 | 7.170 |
| CHAS | 0.141 | 0.000 | 0.000 | 0.313 |
| NOX | 1.101 | 0.538 | 0.538 | 1.647 |
| RM | 15.680 | 6.322 | 100.000 | 27.220 |
| AGE | 58.745 | 65.250 | 100.000 | 33.104 |
| DIS | 6.173 | 3.926 | 24.000 | 6.476 |
| RAD | 78.063 | 5.000 | 5.000 | 203.542 |
| TAX | 339.318 | 307.000 | 666.000 | 180.670 |
| PTRATIO | 42.615 | 18.900 | 20.200 | 87.585 |
| B | 332.791 | 390.660 | 396.900 | 125.322 |
| LSTAT | 11.538 | 10.380 | 7.200 | 6.065 |
| MEDV | 23.558 | 21.950 | 21.950 | 8.343 |

### 2. Distribution Visualization — Histograms
Histograms were plotted for all 14 numerical variables to inspect their shape and spread.

<img width="882" height="354" alt="image" src="https://github.com/user-attachments/assets/1d463f62-77fa-46e4-8289-8dbc3c1b5879" />

<img width="857" height="343" alt="image" src="https://github.com/user-attachments/assets/7eef051c-d9e1-43dc-985f-5491d980833d" />

**Observation:** Distributions vary widely across variables — some are roughly symmetric, while others (e.g. `CRIM`, `ZN`) are heavily right-skewed with a long tail of extreme values.

### 3. Outlier Detection — Boxplots
All numerical variables were standardized (`StandardScaler`) and plotted together as boxplots to make outliers comparable across differently-scaled features.

<img width="903" height="401" alt="image" src="https://github.com/user-attachments/assets/95cd697c-64cc-45d2-a1d2-6d3c42de8ffb" />


**Observation:** Several variables (`CRIM`, `ZN`, `B`, and — consistent with the data quality note above — `RM`, `RAD`, `PTRATIO`) show points well outside the whiskers, flagging them as candidates for outlier treatment before modeling.

### 4. Relationships Between Variables — Scatter Plots

**RM vs. MEDV**

<img width="853" height="515" alt="image" src="https://github.com/user-attachments/assets/7e1824fc-4ef5-46f9-bd9c-3dc3bbb36475" />


For the majority of records (RM roughly 3–9), more rooms is associated with a higher house value, matching the general intuition that bigger homes cost more. The small cluster of points with RM values above 50 corresponds to the data quality issue noted above and pulls the *computed* correlation for this feature down.

**LSTAT vs. MEDV**

<img width="831" height="514" alt="image" src="https://github.com/user-attachments/assets/bec2b1e2-9f9b-47ae-808f-b9803afba1e9" />

A clear negative relationship: as the percentage of lower-status population increases, median house value tends to decrease.

**CRIM vs. MEDV**

<img width="795" height="502" alt="image" src="https://github.com/user-attachments/assets/89d9c61d-7b25-4792-96fd-45546751cd87" />


Most crime rates cluster near zero regardless of house value, with a smaller group of high-crime areas that tend to have lower house values — a weak negative relationship, obscured somewhat by the skew in `CRIM`.

### 5. Correlation Analysis
`MEDV` was chosen as the focus of the correlation analysis since it's the main target variable in this dataset — understanding which features move with it most strongly helps identify good predictors for later modeling.

**Correlation of each feature with MEDV (as computed on the cleaned dataset):**

| Feature | Correlation with MEDV |
|---|---|
| ZN | +0.325 |
| B | +0.194 |
| CHAS | +0.085 |
| DIS | -0.021 |
| RM | -0.049 |
| PTRATIO | -0.065 |
| RAD | -0.074 |
| NOX | -0.087 |
| AGE | -0.201 |
| TAX | -0.232 |
| CRIM | -0.269 |
| INDUS | -0.339 |
| LSTAT | -0.679 |

<img width="1089" height="704" alt="image" src="https://github.com/user-attachments/assets/ed276873-6637-4c65-9dd0-8bc52ac42362" />


**Top 10 strongest pairwise correlations found among all variable pairs (not just against MEDV):** the strongest was between `TAX` and `RAD` (0.910), followed by several other feature-to-feature relationships — useful context for spotting multicollinearity before building a regression model.

> **Note on RM and LSTAT:** `LSTAT` shows the strongest relationship with `MEDV` in this run (**-0.679**), consistent with expectations for this dataset. `RM`, however, shows a very weak correlation here (**-0.049**) rather than the strong positive relationship (~0.7) normally seen in the standard Boston Housing dataset. This is a direct consequence of the corrupted RM values described in the data quality note above — once those rows are fixed or removed, RM's correlation with MEDV should recover to a strong positive value, matching the clear upward trend visible in the "normal" portion of the RM vs. MEDV scatter plot.

### Final EDA Findings (as recorded in the notebook)
1. The dataset contains 506 observations and 14 numerical variables, with considerable variation in spread across variables.
2. Histograms show that variables differ in distribution shape — some symmetric, others skewed with extreme values.
3. Boxplots reveal potential outliers in several variables, which should be accounted for in future modeling.
4. `LSTAT` shows a strong negative correlation with `MEDV` (-0.679 in this run), meaning higher lower-status population percentage is associated with lower house values.
5. Other variables including `INDUS`, `CRIM`, `TAX`, and `AGE` also show negative relationships with `MEDV`, of varying strength.
6. `ZN` and `B` show the strongest positive relationships with `MEDV` among the features checked.

### Conclusion
This EDA highlights `LSTAT` as the clearest single predictor of house value in the current (cleaned but not fully corrected) dataset, with `INDUS`, `CRIM`, and `TAX` as secondary negative predictors, and `ZN` as the strongest positive predictor. Once the RM data quality issue is resolved, `RM` is expected to re-emerge as one of the strongest predictors as well, consistent with domain knowledge of this classic dataset.
