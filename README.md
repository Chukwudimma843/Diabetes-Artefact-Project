# Hospital Readmission Prediction for Diabetic Patients

This project compares three predictive approaches — Logistic Regression, Random Forest, and XGBoost — for predicting 30-day hospital readmission among diabetic patients.
**Author:** Ugo  
**Date:** April 2026

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

- **R version:** 4.3+ recommended  
- **RStudio:** 2023.12+ recommended  
- **Primary packages:**  
- `Hmisc`
- `MASS`
- `assertr`
- `caret`
- `class`
- `corrplot`
- `doParallel`
- `dplyr`
- `flextable`
- `ggplot2`
- `ggpubr`
- `knitr`
- `lme4`
- `lubridate`
- `officer`
- `openxlsx`
- `pROC`
- `patchwork`
- `randomForest`
- `ranger`
- `readr`
- `readxl`
- `tidyverse`
- `vip`

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
   rmarkdown::render("Hospital Readmission Prediction for Diabetic Patients _Ugo Raw.Rmd")
   ```

---

## 🧪 Data & Features

- **Dataset:** Diabetic patient records from multiple U.S. hospitals (1999–2008) — e.g., the UCI *Diabetes 130‑US hospitals* dataset (if applicable).  
- **Target:** 30‑day readmission (yes/no).  
- **Typical predictors:** demographics, admission details, comorbidities, medications, labs, length of stay, discharge disposition, prior utilization, and derived features (ratios/flags).  
- **Pre‑processing:** missing‑value handling, outlier treatment, type coercion (factors vs numeric), data cleaning (e.g., `janitor::clean_names()`), train/test split with `set.seed(...)` for reproducibility.

### Class imbalance
When positive class is rare, consider:
- Stratified train/test split
- Class weights (e.g., `glmnet`, `xgboost`)
- Resampling: up‑sampling, down‑sampling, or SMOTE (`DMwR2::SMOTE()`)

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
- **k‑fold cross‑validation** for tuning hyperparameters.
- **Threshold selection:** based on precision‑recall trade‑offs and cost sensitivity (e.g., maximize F1 or set recall ≥ target).

### Evaluation metrics
- **ROC‑AUC:** overall ranking performance.
- **Sensitivity, Specificity, Precision, Recall, F1‑score.**
- **Confusion matrix** at selected threshold(s).
- **Calibration** (reliability) to ensure predicted probabilities are meaningful.

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

- Compare ROC‑AUC and PR‑AUC across models; for imbalanced data, **PR‑AUC differences are more informative**.
- Inspect feature importances and logistic odds ratios; do they align with clinical expectations?
- Evaluate calibration; if poorly calibrated, consider Platt scaling or isotonic regression.
- Perform error analysis: which subgroups are most misclassified?

---

## 📎 How to Cite

If you use this code or report, please cite the repository and the underlying dataset (e.g., UCI Diabetes 130‑US hospitals). Add formal references in `docs/REFERENCES.md` as needed.

---

## 📄 License

Specify a license (e.g., MIT) in `LICENSE` to clarify usage permissions.

---

## 🤝 Contributing

Issues and pull requests are welcome. Please open a discussion before major changes.

---

## 🗒 Notes

- This README is auto‑generated based on your Rmd; adjust dataset details and paths where needed.
