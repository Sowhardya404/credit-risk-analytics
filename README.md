# Credit Risk Analytics & Default Prediction System

End-to-end machine learning pipeline for loan default prediction, risk segmentation, 
and expected loss estimation — with an interactive Power BI dashboard for business stakeholders.

---

## Project Overview

This project builds a production-style credit risk system that:

- Predicts whether a borrower will default on a loan using supervised ML
- Segments borrowers into **Low / Medium / High Risk** tiers based on predicted probability
- Calculates **Expected Loss in USD** per borrower (Basel III EL = PD × LGD × EAD framework)
- Explains model predictions using **SHAP values** (industry standard for regulated environments)
- Exports scored data to **Power BI** for executive-level visual reporting

The system is designed to mirror real-world credit underwriting workflows — with explicit 
handling of data leakage, class imbalance, and business-interpretable outputs.

---

## Tech Stack

| Layer | Tools |
|---|---|
| Language | Python 3 |
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-learn (Logistic Regression, Random Forest), XGBoost |
| Model Explainability | SHAP |
| Visualisation (Python) | Matplotlib, Seaborn |
| Business Dashboard | Microsoft Power BI |
| Notebook Environment | Jupyter Notebook |
| Data Format | CSV (Loan_Default.csv) |

---

## Dataset

**Source:** [Loan Default Dataset — Kaggle](https://www.kaggle.com/datasets/yasserh/loan-default-dataset)  
148,670 loan applications · 34 features · 24.6% default rate

> Download `Loan_Default.csv` and place it in the `/data` folder before running.

---

## How to Run

### 1. Python Notebook (ML Pipeline)

**Requirements:** Python 3.8+, or use Google Colab (recommended)

```bash
pip install pandas numpy scikit-learn xgboost shap matplotlib seaborn
```

**Steps:**

1. Place `Loan_Default.csv` in `/data/` (or update the path in Cell 3)
2. Open `Credit_Risk_Analytics_System.ipynb`
3. Run all cells top to bottom — outputs will be saved as:
   - `credit_risk_scores.csv` — per-borrower risk scores and expected loss
   - `risk_tier_summary.csv` — aggregated tier statistics

### 2. Power BI Dashboard

1. Open `Loan_Prediction.pbix` in Power BI Desktop
2. Go to **Transform Data → Data Source Settings**
3. Update the file path to point to your local `credit_risk_scores.csv` and `risk_tier_summary.csv`
4. Click **Refresh** — all KPI cards and charts will update automatically

---

## Methodology

### Data Pipeline

- **Missing values:** Numeric → median imputation; Categorical → mode imputation
- **Outlier removal:** IQR-based filtering on `income`, `loan_amount`, `Credit_Score`
- **Encoding:** One-hot encoding for categorical features
- **Data leakage prevention:** Post-approval variables (`Interest_rate_spread`, `rate_of_interest`, 
  `Upfront_charges`) explicitly excluded from model features

### Models Trained

| Model | Notes |
|---|---|
| Logistic Regression | Interpretable baseline; `class_weight='balanced'` |
| Random Forest | Captures non-linear interactions; depth-constrained to prevent overfitting |
| XGBoost | Gradient boosting; `scale_pos_weight` for class imbalance; early stopping |

- **Train/test split:** 80/20, stratified
- **Scaling:** `StandardScaler` fit on train only (no leakage)
- **Validation:** 5-fold stratified cross-validation via `Pipeline`

### Risk Segmentation

| Tier | Default Probability | Recommended Action |
|---|---|---|
| Low Risk | < 30% | Approve; offer competitive rates |
| Medium Risk | 30% – 65% | Approve with standard terms |
| High Risk | > 65% | Decline or require additional collateral |

### Expected Loss

$$EL = PD \times LGD \times EAD$$

> *Simplification: LGD = 100%, EAD = original loan amount. This is a conservative upper-bound estimate. 
> A production model would use historical recovery rates (typically 30–60% for secured mortgages).*

---

## Results

| Metric | Logistic Regression | Random Forest | XGBoost |
|---|---|---|---|
| Accuracy | ~81.7% | ~85.9% | ~86.8% |
| ROC-AUC | ~0.82 | ~0.85 | **~0.87** |
| CV AUC (5-fold) | 0.814 ± 0.002 | 0.852 ± 0.003 | **0.870 ± 0.003** |

**Top predictive features (SHAP):** LTV ratio, Credit Type (EQUI), Debt-to-Income ratio, 
Property Value, Income

**Business impact:**

- Proactive identification of high-risk borrowers before loan approval
- Expected Loss estimates support capital reserve planning (Basel III aligned)
- Risk tier dashboard allows credit officers to prioritise reviews efficiently
- SHAP explanations provide audit-ready reasoning per individual prediction

---

## Power BI Dashboard

The `.pbix` dashboard includes:

- **KPI cards:** Total borrowers, High Risk count, average default probability, total expected loss
- **Scatter chart:** Default probability vs loan amount, coloured by risk tier
- **Column charts:** Borrower distribution by risk tier; actual default rate by tier
- **Slicer:** Filter by risk level interactively

---

## Author

**Sowhardya Biswas**  

Email:sowhardya.biswas2003@gmail.com

---

## License

This project is for educational and portfolio purposes only.

