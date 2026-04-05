# Credit Risk Analytics & Default Prediction System

> End-to-end machine learning pipeline for loan default prediction, risk segmentation, and expected loss estimation — with an interactive Power BI dashboard for business stakeholders.

---

## Project Overview

This project builds a production-style credit risk system that:

1. Predicts whether a borrower will default on a loan using supervised ML
2. Segments borrowers into **Low / Medium / High Risk** tiers based on predicted probability
3. Calculates **Expected Loss in USD** per borrower 
4. Exports scored data to Power BI for executive-level visual reporting

The system is designed to mirror real-world credit underwriting workflows — with explicit handling of data leakage, class imbalance, and business interpretable outputs.

---

## Tech Stack

| Layer | Tools |
|---|---|
| Language | Python 3 |
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-learn (Logistic Regression, Random Forest) |
| Visualisation (Python) | Matplotlib, Seaborn |
| Business Dashboard | Microsoft Power BI |
| Notebook Environment | Google Colab |
| Data Format | CSV (Loan_Default.csv) |

---

## Project Structure

```

credit-risk-analytics/
│
├── Credit_Risk_Analytics_System.ipynb   # Full ML pipeline (EDA → Model → Risk → Export)
├── Loan_Prediction.pbix                # Power BI dashboard (loads exported CSVs)
│
├── data/
│   └── Loan_Default.csv               # Input dataset (place here)
│
├── final_outcome/
│   ├── credit_risk_scores.csv        # Per-borrower predictions + risk tier
│   └── risk_tier_summary.csv         # Aggregated KPI table for Power BI
│
├── requirements.txt                  # Python dependencies for the project
└── README.md
```

---

## How to Run

### 1. Python Notebook (ML Pipeline)

**Requirements:** Python 3.8+, or use Google Colab (recommended)

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```

Steps:

1. Place `Loan_Default.csv` in `/content/` (Colab) or update the path in Cell 2
2. Open `Credit_Risk_Analytics_System.ipynb`
3. Run all cells top to bottom — outputs will be saved as:
   - `credit_risk_scores.csv` — per-borrower risk scores
   - `risk_tier_summary.csv` — aggregated tier statistics

### 2. Power BI Dashboard

1. Open `Loan_Prediction.pbix` in **Power BI Desktop**
2. Go to **Transform Data → Data Source Settings**
3. Update the file path to point to your local `credit_risk_scores.csv` and `risk_tier_summary.csv`.
4. Click **Refresh** — all KPI cards and charts will update automatically

---

## Methodology

### Data Pipeline

- **Missing values:** Numeric → median imputation; Categorical → mode imputation
- **Outlier removal:** IQR-based filtering on `income`, `loan_amount`, `Credit_Score`
- **Encoding:** One-hot encoding for categorical features
- **Data leakage prevention:** Post-approval variables (`Interest_rate_spread`, `rate_of_interest`, `Upfront_charges`) excluded from features

### Models Trained

| Model | Notes |
|---|---|
| Logistic Regression | Interpretable baseline; `class_weight='balanced'` |
| Random Forest | Captures non-linear interactions; depth-constrained to prevent overfitting |

- Train/test split: 80/20, stratified
- Scaling: `StandardScaler` fit on train only (no leakage)
- Validation: 5-fold stratified cross-validation

### Risk Segmentation

| Tier | Default Probability |
|---|---|
| Low Risk | < 30% |
| Medium Risk | 30% – 65% |
| High Risk | > 65% |

---

## Results

| Metric | Logistic Regression | Random Forest |
|---|---|---|
| Accuracy | ~81.66% | ~85.88% |
| ROC-AUC | ~0.82 | ~0.85 |
| CV AUC (5-fold) | Stable (low std) | Stable (low std) |

**Top predictive features:** Loan-to-Value ratio (LTV), Credit Type (EQUI), Debt-to-Income ratio (DTI), Property Value, Income

**Business impact:**

- Enables proactive identification of high-risk borrowers before loan approval
- Expected Loss estimates support capital reserve planning
- Risk tier dashboard allows credit officers to prioritise reviews efficiently

---

## Power BI Dashboard

The `.pbix` dashboard includes:

- KPI cards: Total borrowers, High Risk count, average default probability, total expected loss
- Scatter chart: Default probability vs loan amount, coloured by risk tier
- Column charts: Borrower distribution by risk tier; default rate by tier
- Slicer: Filter by risk level interactively

---

## Author

**Sowhardya Biswas**

- Email: `sowhardya.biswas2003@gmail.com`

---

## License

This project is for educational and portfolio purposes.
