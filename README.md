# Hospital Readmission Prediction for Diabetic Patients

Predicting 30‑day hospital readmissions among diabetic patients using statistical and machine‑learning approaches in R.

**Author:** Ugo  
**Date:** August 2025

---

## 📌 Project Overview

This repository contains the full technical and statistical workflow for building and evaluating models to predict near‑term hospital readmission in diabetic patients. The analysis is implemented in R via an R Markdown report, with reproducible steps for data preparation, exploratory analysis, feature engineering, model development, and evaluation.

**Core approaches:** anova, auc, confusion matrix, logistic regression, precision, random forest, recall, roc.

---

## 🗂 Repository Structure

```text
.
├── data/                  # (optional) Raw/processed data (excluded from git if sensitive)
├── scripts/               # Utility R scripts (if you split the Rmd later)
├── models/                # Saved model objects / tuning artifacts
├── figs/                  # Exported plots (ROC, PR, importances)
├── reports/               # Rendered outputs (HTML/PDF/Word)
├── docs/                  # Additional methodological notes
├── Hospital Readmission Prediction for Diabetic Patients _Ugo Raw.Rmd
└── README.md
```

> Tip: Add `data/` and any secrets to `.gitignore` if needed.

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

### Machine learning models
- **Random Forest:** non‑linear interactions, robust to noise; tune trees (`ntree`), depth, and `mtry` via cross‑validation.
- **XGBoost:** gradient boosting on decision trees; tune `eta`, `max_depth`, `min_child_weight`, `subsample`, `colsample_bytree`, `lambda`, `alpha`, `nrounds` with k‑fold CV.
- **Regularized GLM (optional):** LASSO / Elastic Net via `glmnet` for embedded feature selection.

### Validation strategy
- **Train/test split** with fixed `set.seed(...)` for reproducibility.
- **k‑fold cross‑validation** for tuning hyperparameters.
- **Threshold selection:** based on precision‑recall trade‑offs and cost sensitivity (e.g., maximize F1 or set recall ≥ target).

### Evaluation metrics
- **ROC‑AUC:** overall ranking performance.
- **Precision–Recall curve & AUC:** preferred for imbalanced targets; emphasizes positive class.
- **Sensitivity, Specificity, Precision, Recall, F1‑score.**
- **Confusion matrix** at selected threshold(s).
- **Calibration** (reliability) to ensure predicted probabilities are meaningful.

### Explainability
- **Global:** permutation/GINI importances (RF), gain/cover (XGB).
- **Local (optional):** coefficients for logistic regression; consider partial‑dependence profiles or accumulated local effects if added later.

---

## 📊 Key Outputs

The Rmd renders and/or saves:
- ROC and PR curves comparing models
- Confusion matrices & classification reports
- Feature importance plots
- Calibration plots (if enabled)
- Exported artifacts (e.g., `models/`, `figs/`, `reports/`)

---

## 🧾 Reproducibility

- Set random seeds with `set.seed(123)` (replace 123 with your chosen seed).
- Document data provenance; avoid committing sensitive data.
- Record package versions: `sessionInfo()` output in the report.

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
