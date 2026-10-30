# Telecom-Customer-Churn-Analysis
End-to-end telecom churn analysis: Python EDA, KPIs and ML model (scikit-learn) with a 4-page Power BI dashboard. 26.5% churn, $139K MRR lost per month
# Telecom Customer Churn Analysis: Python, Machine Learning and Power BI

> **Business question:** Which customers are leaving, why, how much revenue does it cost, and who should the retention team contact first?

![Dashboard overview](dashboard/page1_overview.png)

## Project summary
An end-to-end churn analysis on the public IBM Telco Customer Churn dataset (7,043 customers). The project cleans and explores the data in Python, builds KPIs for revenue impact, trains a churn prediction model, and delivers a 4-page Power BI dashboard with a ranked retention action list.

| | |
|---|---|
| **Tools** | Python (pandas, NumPy, matplotlib, seaborn, scikit-learn), Jupyter Notebook, Power BI (DAX, Power Query) |
| **Data** | IBM Telco Customer Churn (public). Snapshot date: 2019-09-30 |
| **Output** | Analysis notebook, ML model, cleaned dataset, Power BI dashboard |

> **Data note:** Some columns (complaints, escalations, CSAT, subscription type, cancellation dates) were simulated to extend the original dataset. No conclusions are drawn from them. All findings use the original IBM fields.

## Key findings
Snapshot KPIs from the notebook (7,043 customers):

| KPI | Value |
|---|---|
| Churn rate / retention rate | 26.5% / 73.5% |
| ARPU (monthly) | $64.76 |
| Total MRR | $456,117 |
| MRR lost to churn | $139,131 (30.5% of MRR) |
| Annualised revenue lost | $1,669,570 |
| MRR at risk (active, high-risk tier) | $57,568 |
| CLTV lost | $7,755,256 |
| Average tenure, churned vs retained | 18.0 vs 37.6 months |

1. **Churn costs $139K a month.** That is 30.5% of MRR, or about **$1.67M a year**. Churners pay more than the average customer, so revenue loss (30.5%) is higher than customer loss (26.5%).
2. **Contract type is the strongest driver.** Month-to-month customers churn at about 43%, against about 11% on one-year and 3% on two-year contracts.
3. **Customers leave early.** Churned customers had an average tenure of 18.0 months, against 37.6 months for retained customers.
4. **Service profile matters.** Customers with [fiber optic / no tech support / electronic check] churn at 0.42%, compared with 0.45% for the rest.
5. **Top cancellation reasons** account for 29% of lost MRR: Competitor Offered Higher Dowanlaod Speeds, Attitude of support person, Competitor Offered More Data.
6. **$57,568 of MRR is at risk** among active customers flagged high risk.

## Machine learning model
- **Goal:** rank active customers by probability of churn.
- **Models compared:** Logistic Regression, Random Forest, Gradient Boosting (5-fold stratified cross-validation).
- **Best model:** Gradient Boosting, ROC-AUC 0.84, recall on churners 0.64 at threshold 0.4.
- **Features used:** tenure, monthly charges, contract, internet service, payment method, tech support, senior citizen, dependents.
- **Leakage control:** `churn_score`, `churn_risk`, `cltv`, cancellation fields and simulated columns are excluded from the model.
- **Class imbalance:** handled with class weights; evaluated on recall and AUC, not accuracy.

## Power BI dashboard
| Page | What it shows |
|---|---|
| **1. Overview** | Customers, churn rate, ARPU, total MRR, MRR lost, MRR at risk, and churn by contract, tenure and plan |
| **2. Churn Drivers** | Pareto chart of cancellation reasons, plan x contract heatmap, risk tiers |
| **3. Profile** | Churn by internet service, payment method, tech support, senior citizen and dependents |
| **4. Action List** | Top 50 active customers ranked by churn probability, with MRR at risk |

Key DAX used: `CALCULATE`, `DIVIDE`, `DISTINCTCOUNT`, `ALL`, `SUMX` with `FILTER` (for the cumulative Pareto line).

## How I used AI
I used an AI assistant (Claude) as a reviewer and coding partner, not as a replacement for the analysis:
- **Review:** asked it to critique the first version of the notebook as a hiring analyst would. It flagged leaked and simulated columns, wrong KPI definitions, and a correlation run on only 5 rows.
- **Code:** used it to draft the KPI, survival-curve and model code, then ran every cell myself and fixed errors.
- **Power BI:** used it to explain DAX measures such as `CALCULATE`, `ALL` and the cumulative Pareto formula, and checked each one against my own numbers.
- **Checks I did myself:** reconciled the KPIs between notebook and dashboard, read the data dictionary, and kept simulated columns out of the conclusions.

## Recommendations
1. Offer incentives to move month-to-month customers to annual contracts in their first 6 months.
2. Bundle tech support for high-risk fiber customers.
3. Encourage auto-pay over electronic check.
4. Give the retention team the ranked action list and review it monthly.

## Repository structure
```
telecom-customer-churn-analysis/
├── Telecom_churn_improved.ipynb   # full analysis and ML model
├── churn_clean.csv                # cleaned dataset used by Power BI
├── dashboard/
│   ├── Telecom_Churn_Dashboard.pbix
│   └── page1_overview.png ...     # one screenshot per page
├── requirements.txt
└── README.md
```

## How to run
1. Clone the repo and install the packages:
   ```bash
   pip install -r requirements.txt
   ```
2. Place the source data files (`churn_data.csv` and the raw Telco columns file) in the project folder.
3. Open `Telecom_churn_improved.ipynb` and choose **Kernel -> Restart & Run All**.
4. The notebook exports `churn_clean.csv`. Open the `.pbix` file in Power BI Desktop and click **Refresh**.

`requirements.txt`:
```
pandas
numpy
matplotlib
seaborn
scikit-learn
openpyxl
```

## Method
1. **Data quality:** nulls, duplicates, and structural nulls in the cancellation fields.
2. **Feature engineering:** tenure buckets, risk tiers, MRR lost.
3. **KPI scorecard:** churn, retention, ARPU, MRR, MRR lost, MRR at risk, CLTV lost.
4. **Segment analysis:** contract, plan, tenure, Pareto of cancellation reasons, Kaplan-Meier survival curve.
5. **Drivers of churn:** correlations on the full dataset.
6. **Predictive model:** model comparison, ROC curve, feature importance, threshold selection, action list.
7. **Dashboard:** four-page Power BI report.

## Limitations
- Simulated columns are excluded from conclusions.
- `plan_type` is derived from monthly charge bands.
- `churn_score` and `churn_risk` are model outputs. They are used to prioritise customers, not to explain churn.
- The data is a single snapshot, and the model is not tuned or monitored over time.
- Correlation is not causation. An A/B test of retention offers would be needed to confirm impact.

## Data source
IBM Telco Customer Churn dataset, a public sample dataset from IBM.

## Author
**Manish** | Data Analyst
Skills: Python · SQL · Power BI · Excel · Streamlit
[LinkedIn](https://www.linkedin.com/in/your-profile) · [GitHub](https://github.com/your-username)
