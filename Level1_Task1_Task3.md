# Task 1: Data Cleaning and Preprocessing — Analysis Summary

**Dataset:** `house Prediction Data Set.csv` (Boston Housing dataset — 506 records, 14 columns)
**Tools:** Python, pandas, numpy

---

## Objective

Take a raw dataset containing missing values, potential duplicates, and inconsistent formatting, and produce a clean, analysis-ready version using pandas.

## Methodology

The google colab notebook works through the cleaning process in five steps:

### 1. Load the Dataset
The CSV was loaded with `pandas.read_csv()`. The raw data starts at **506 rows and 14 columns**, covering features like crime rate (`CRIM`), average rooms (`RM`), pupil-teacher ratio (`PREATIO`), and the target price column (`MEDV`).

### 2. Inspect Data Types and Missing Values
`df.info()` and `df.isnull().sum()` were used to check each column's data type and completeness. This revealed:

| Column | Non-Null Count | Missing |
|---|---|---|
| `MEDV` (target price) | 452 / 506 | **54 missing values** |
| All other 13 columns | 506 / 506 | 0 missing |

Only the target column had gaps — every feature column was fully populated.

### 3. Handle Duplicate Rows
`df.duplicated().sum()` was used to check for exact duplicate rows.

**Result:** 0 duplicates found — no rows needed to be removed.

### 4. Handle Missing Values (Imputation)
Columns were split into numerical and categorical groups using `select_dtypes()`. For any numerical column with missing values, the **median** was used to fill gaps (median was chosen over mean because it's less sensitive to outliers).

**Result:** The 54 missing values in `MEDV` were filled with the column median (**21.95**). No categorical columns required imputation, since none had missing values in this dataset.

### 5. Standardize Data Formats
All categorical/text columns were cleaned by:
- Stripping leading/trailing whitespace
- Converting to consistent title-case capitalization

A check was also included to convert any `date` column to a standard datetime format, though this particular dataset doesn't contain a date column, so that step was skipped automatically.

### 6. Final Data Summary
After cleaning:

| Metric | Value |
|---|---|
| Final shape | 506 rows × 14 columns |
| Remaining missing values | **0** |
| Duplicate rows removed | 0 |
| Values imputed | 54 (in `MEDV`, via median) |

## Conclusion

The dataset was fully cleaned and validated: every missing value was resolved, no duplicate records existed, and text formatting was standardized. The dataset went from **452 usable rows (before imputation)** to a **complete 506-row dataset** ready for exploratory analysis or modeling — no rows had to be dropped, since imputation preserved every record.


