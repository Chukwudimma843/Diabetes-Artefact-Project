# Statistical & Technical Notes

This note expands on the **direct** statistical reasoning and technical design used in the project.

## 1) Problem Definition
Binary classification: predict **30‑day readmission** (yes/no) for diabetic inpatients.

## 2) Data Pipeline
1. **Ingest & Clean:** Import CSV/Excel; clean names, coerce types, harmonize missing codes.
2. **Feature Engineering:** One‑hot encode categoricals, derive counts/ratios (e.g., meds, prior visits), flag critical labs, compute length‑of‑stay bins.
3. **Train/Test Split:** Stratified split (e.g., 70/30) with `set.seed(...)`.
4. **Imbalance Handling:** Try class weights (logistic/XGB), up‑sampling, down‑sampling, or SMOTE.
5. **Standardization:** Scale numeric features when needed (e.g., for GLM/regularized models).

## 3) Baseline Statistical Modelling
- **Univariate testing:**  
  - Numeric → ANOVA/t‑test vs target; Categorical → chi‑squared association.  
  - Controls: multiple‑testing awareness (e.g., Benjamini–Hochberg FDR if screening many features).
- **Logistic regression:**  
  - Fit `readmitted ~ predictors`; check VIF for multicollinearity.  
  - Report coefficients, ORs (= exp(beta)), 95% CIs, and p‑values.  
  - Add interactions only if clinically motivated.  
  - Diagnostics: calibration curve, Hosmer–Lemeshow (with caution), residuals.

## 4) Machine Learning Models
- **Random Forest:**  
  - Tunables: `ntree`, `mtry`, `nodesize`, `maxnodes`.  
  - Strengths: non‑linearities, interactions, robustness.  
  - Outputs: GINI/permutation importances.
- **XGBoost:**  
  - Tunables: `eta`, `max_depth`, `min_child_weight`, `gamma`, `subsample`, `colsample_bytree`, `lambda`, `alpha`, `nrounds`.  
  - Early stopping and k‑fold CV recommended.  
  - Outputs: gain/cover importances.
- **Regularized GLM (optional):**  
  - LASSO/Elastic Net (`glmnet`) for sparse, interpretable models.  
  - Select `alpha` and `lambda` via CV.

## 5) Evaluation
- **Primary metrics:** PR‑AUC (imbalanced), ROC‑AUC (global).  
- **Thresholded metrics:** sensitivity, specificity, precision, recall, F1.  
- **Calibration:** reliability curve, Brier score.  
- **Reporting:** confusion matrix and classification report for chosen threshold(s).

## 6) Model Selection & Interpretation
- Prefer the model with **best PR‑AUC** subject to clinical constraints (e.g., minimum recall).  
- Inspect feature importances and odds ratios; highlight clinically plausible drivers (e.g., prior admissions, insulin changes, comorbidities).  
- Document limitations: dataset era (1999–2008), site heterogeneity, missing SDOH, potential coding bias.

## 7) Reproducibility
- `set.seed(...)` for all stochastic steps.  
- Pin package versions; include `sessionInfo()` in report.  
- Keep PHI out of the repo; store raw data securely; commit only derived non‑identifiable summaries.

---

**File of interest:**  
- `Hospital Readmission Prediction for Diabetic Patients _Ugo Raw.Rmd` – full, executable analysis.
