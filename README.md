# Hospital Readmission Prediction for Diabetic Patients

This project compares three predictive approaches — Logistic Regression, Random Forest, and XGBoost — for predicting 30-day hospital readmission among diabetic patients.
**Author:** Ugochukwu chukwudimma  
**Date:** updated in April 2026

---

## 📌 Project Overview

This repository contains the full technical and statistical workflow for building and evaluating models to predict near‑term hospital readmission in diabetic patients. The analysis is implemented in R via an R Markdown report, with reproducible steps for data preparation, exploratory analysis, feature engineering, model development, and evaluation.

**Core approaches:** ANOVA, AUC, logistic regression, precision, random forest, XGBoost, recall,Specificity,F1-score and ROC analysis.
---

## 🗂 Repository Structure

├── data/
├── models/
├── figs/
├── reports/
├── docs/
├── Re_admission Analysis2.Rmd
└── README.md
---

## 🔧 Environment & Setup

### Requirements

- **R version:** 4.3+ recommended  
- **RStudio:** recommended for running and knitting the R Markdown file  

### Packages Used

The analysis relies on a range of packages for data import, preprocessing, visualisation, statistical analysis, model training, evaluation, and reporting.

- `openxlsx`
- `readxl`
- `readr`
- `doParallel`
- `vip`
- `Hmisc`
- `corrplot`
- `dplyr`
- `ggplot2`
- `knitr`
- `class`
- `lme4`
- `MASS`
- `tidyverse`
- `assertr`
- `ggpubr`
- `lubridate`
- `flextable`
- `patchwork`
- `randomForest`
- `pROC`
- `officer`
- `caret`

### Install Required Packages

```r
required_pkgs <- c(
  "openxlsx", "readxl", "readr", "doParallel", "vip",
  "Hmisc", "corrplot", "dplyr", "ggplot2", "knitr",
  "class", "lme4", "MASS", "tidyverse", "assertr",
  "ggpubr", "lubridate", "flextable", "patchwork",
  "randomForest", "pROC", "officer", "caret"
)

to_install <- setdiff(required_pkgs, rownames(installed.packages()))
if (length(to_install)) {
  install.packages(to_install, repos = "https://cloud.r-project.org")
}

### Install packages
You can install any missing packages with:
```r
required_pkgs <- c("Hmisc", "MASS", "assertr", "caret", "class", "corrplot", "doParallel", "dplyr", "flextable", "ggplot2", "ggpubr", "knitr", "lme4", "lubridate", "officer", "openxlsx", "pROC", "patchwork", "randomForest", "ranger", "readr", "readxl", "tidyverse", "vip")
to_install <- setdiff(required_pkgs, rownames(installed.packages()))
if (length(to_install)) install.packages(to_install, repos = "https://cloud.r-project.org")
```

---

## ▶️ How to Reproduce

1. Clone this repository:
   ```bash
   git clone https://github.com/YOUR-USER/YOUR-REPO.git
   cd YOUR-REPO
   ```
2. Open the R Markdown file in RStudio:  
   `Hospital Readmission Prediction for Diabetic Patients _Ugo Raw.Rmd`
3. (Optional) Place your dataset(s) under `data/` and update paths in the Rmd.
4. Click **Knit** to render to HTML/PDF/Word.  
   Or run:
   ```r
   rmarkdown::render("Re_admission Analysis2.Rmd")
   ```

---

## 🧪 Data & Features

- **Dataset:** Diabetic patient records from 130 U.S. hospitals (1999–2008), based on the UCI diabetes readmission dataset.
- **Target:** 30-day hospital readmission, recoded as a binary outcome:
  - `1` = readmitted within 30 days (`<30`)
  - `0` = not readmitted within 30 days

### Predictors

The analysis used a selected set of demographic, clinical, utilisation, and medication-related variables, including:

- Demographics: `race`, `age`
- Hospital utilisation: `time_in_hospital`, `number_inpatient`, `number_outpatient`, `number_emergency`
- Clinical activity: `num_procedures`, `num_lab_procedures`, `number_diagnoses`
- Admission and discharge details: `admission_type_id`, `discharge_disposition_id`
- Laboratory indicators: `A1Cresult`, `max_glu_serum`
- Medication-related variables: `change`, `repaglinide`, `glipizide`, `insulin`, `metformin`, `diabetesMed`

### Pre-processing

The following preprocessing steps were applied:

- The target variable `readmitted` was transformed into a binary classification outcome
- Variables with high missingness were removed, including:
  - `weight`
  - `payer_code`
  - `medical_specialty`
- Non-informative identifiers and single-value variables were removed, including:
  - `encounter_id`
  - `patient_nbr`
  - `examide`
  - `citoglipton`
- Missing values were imputed:
  - numeric variables using the **mean**
  - categorical variables using the **mode**
- Ordered categorical encoding was applied to:
  - `max_glu_serum`
  - `A1Cresult`
- Medication variables were standardised into grouped categorical form
- A modelling subset of significant variables (`df_sig`) was created for downstream analysis
- The dataset was split into training and test sets using a fixed random seed for reproducibility

### Class Imbalance Handling

Because 30-day readmission was the minority class, the project addressed class imbalance using:

- **SMOTE** for Random Forest training
- **Class weighting** for XGBoost
- Evaluation with metrics such as Recall, Specificity, F1 Score, and AUC

---

## 📐 Statistical & ML Methods (Direct Explanation)

### Statistical framing
- **Univariate screening:** t‑tests / ANOVA for numeric features and chi‑squared tests for categorical features to identify variables associated with readmission.
- **Logistic regression (baseline):** interpretable odds‑ratio estimates; check multicollinearity (VIF), linearly separable effects, interaction terms as needed.
- **Model diagnostics:** residual analysis, calibration (e.g., calibration curve, Brier score), and threshold analysis.

## 🤖 Machine Learning Models

Three predictive models were developed and compared in this project:

### 1. Logistic Regression
- Used as a baseline statistical model
- Suitable for binary classification
- Provides interpretable coefficients and helps explain how predictors influence readmission risk

### 2. Random Forest
- An ensemble tree-based model
- Captures non-linear relationships and feature interactions
- Useful for handling complex healthcare data with mixed variable types

### 3. XGBoost
- A gradient boosting model designed for high predictive performance
- Handles complex patterns, non-linearity, and interactions effectively
- Often performs well on structured/tabular datasets such as hospital readmission data
- Included in this project to compare a more advanced boosting approach against Logistic Regression and Random Forest

---

## 📊 Model Evaluation

The three models were evaluated and compared using:

- ROC-AUC
- Confusion Matrix
- Accuracy
- Precision
- Recall
- Specificity
- F1 Score

This comparison helped assess not only overall predictive performance, but also how well each model identified patients at risk of 30-day readmission.

### Validation strategy
- **Train/test split** with fixed `set.seed(...)` for reproducibility.

- **Threshold selection:** based on precision‑recall trade‑offs and cost sensitivity (e.g., maximize F1 or set recall ≥ target).

### Evaluation metrics
- **ROC‑AUC:** overall ranking performance.
- **Sensitivity, Specificity, Precision, Recall, F1‑score.**
- 
  

### Explainability
- **Global:** permutation/GINI importances (RF), gain/cover (XGB).
- **Local (optional):** coefficients for logistic regression; consider partial‑dependence profiles or accumulated local effects if added later.

---

## 📈 Key Outputs

- Missing data summary
- EDA plots
- Statistical test results
- Correlation analysis
- Train/test split summary
- SMOTE class-balance output
- Logistic Regression results
- Random Forest results
- XGBoost results
- ROC curve
- Final model comparison table

## 🔁 Reproducibility

The analysis is designed to be fully reproducible.

- A fixed random seed (`set.seed(123)`) ensures consistent train/test splits and model results  
- All steps, including preprocessing, modeling, and evaluation, are contained within a single R Markdown file  
- Required packages are explicitly listed for environment setup  

To reproduce the results:

1. Clone the repository  
2. Add the dataset to the `data/` directory  
3. Open and run the R Markdown file  

Session information can be obtained using:

```r
sessionInfo()

---

## 🧠 Interpreting Results (What to look for)

 The results should be interpreted based on how well each model identifies patients at risk of 30-day readmission.
Key points to consider include:
Compare the performance of Logistic Regression, Random Forest, and XGBoost using Accuracy, Precision, Recall, Specificity, F1 Score, and AUC
Use ROC-AUC to assess how well each model distinguishes between readmitted and non-readmitted patients
Use the confusion matrix to understand the number of correct and incorrect classifications
Pay close attention to Recall (Sensitivity), as this is especially important in healthcare settings where missing a high-risk patient can have serious consequences
Consider Specificity alongside recall to assess how well the model avoids false alarms
Review the threshold comparison results, especially for XGBoost, to understand how threshold selection affects classification performance
Overall, the results should be interpreted not only in terms of overall model performance, but also in terms of clinical usefulness and the ability to correctly identify patients at higher risk of readmission.
---

## 📎 How to Cite
If you use this project, please cite:
this repository
the dataset used in the analysis (for example, the UCI Diabetes 130-US hospitals dataset, if applicable)
Formal references can also be added in a separate references file if needed.
---

## 📄 License

No license has been specified for this project yet.
---

## 🤝 Contributing

Contributions, suggestions, and improvements are welcome.

If you would like to contribute, please open an issue or start a discussion to propose changes before submitting a pull request.

---

## 🗒 Notes

This project presents a full workflow for predicting 30-day hospital readmission in diabetic patients using R
The analysis includes data preprocessing, exploratory analysis, statistical testing, model development, and model evaluation
The project compares three models: Logistic Regression, Random Forest, and XGBoost
