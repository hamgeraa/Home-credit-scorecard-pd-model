# Home-credit-scorecard-pd-model

# Credit Default Prediction — Scorecard-Style PD Model (Home Credit)

## TL;DR
- Built a **scorecard-style Probability of Default (PD)** model using **WOE + Logistic Regression** on the Home Credit dataset
- Validated using **ROC-AUC / KS** and **decile bad-rate separation**
- Converted the PD model into a **points-based scorecard** using **PDO/base score scaling** (points per bin)

---

## Business Problem
Lenders need interpretable risk models to rank applicants by default risk and support decisions such as:
- approve/decline cutoffs
- risk-based pricing
- portfolio risk monitoring

This project builds a **traditional scorecard pipeline** (WOE/IV + Logistic Regression) and outputs a **points-per-bin scorecard**.

---

## Dataset
**Home Credit Default Risk (Kaggle)**  
- `application_train.csv` (main table; one row per applicant)
- Optional add-on: `bureau.csv` (credit bureau history; aggregated to applicant level)
- Target: `TARGET` (1 = default, 0 = non-default)

> Note: Raw datasets are not included in this repo. Download from Kaggle and place files in `data/raw/`.

---

## Method Overview (Scorecard Pipeline)
1) **Baseline model:** numeric-only Logistic Regression (fast benchmark)
2) **(Optional) Bureau add-on:** aggregate bureau features → merge → compare lift
3) **Binning:** quantile bins + explicit **Missing** bin
4) **WOE / IV:** compute WOE per bin + IV per feature (feature selection + interpretability)
5) **WOE model:** train Logistic Regression on WOE-transformed features
6) **Score scaling:** PD → score using **PDO/base score** and generate **points per bin**
7) **Validation:** AUC/KS + decile separation + cutoff example

---

## Results (Validation)
> Fill these in with your final numbers.
- **Baseline LR (numeric):** AUC = ____, KS = ____
- **WOE Scorecard LR:** AUC = ____, KS = ____
- **Decile separation:** Top decile bad rate = ____% vs Bottom decile = ____%

---

## Visuals
### ROC Curve
![ROC](figures/roc_curve.png)

### Bad Rate by Predicted Risk Decile
![Deciles](figures/bad_rate_by_decile.png)

---

## Key Artifacts
- `outputs/iv_summary.csv` — feature strength via IV ranking  
- `outputs/scorecard_points_table.csv` — **Feature × Bin → Points** (final scorecard table)  
- `outputs/validation_deciles.csv` — decile bad-rate table (credit-style separation)

---

## How to Run
1) Install dependencies:
```bash
pip install -r requirements.txt

