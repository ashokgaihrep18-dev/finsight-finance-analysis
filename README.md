# 📊 FinSight — Finance Analysis Dashboard

> **Real-time insights into transactions, customer behaviour, risk exposure, and fee performance.**  
> Built with Power BI · 50,069 transactions · 5,000 customers · Dynamic slicers · YoY KPIs

![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-blue?style=flat)
![CSV](https://img.shields.io/badge/Data-CSV-brightgreen?style=flat)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat)

---

## 🖼️ Dashboard Preview

| Overview Analysis | Transactions Drillthrough |
|:---:|:---:|
| Overview <img width="1300" height="689" alt="image" src="https://github.com/user-attachments/assets/7d1abbca-4150-461d-8584-9c2575f8efa6" />
| ![Transactions]
(<img width="645" height="340" alt="image" src="https://github.com/user-attachments/assets/1f634872-b8ab-40f5-85a7-3f910b27e8c5" />
 |
| KPIs · Monthly trend · Geo breakdown · Type table | Row-level transaction detail · Sortable columns |

---

## 📌 Project Summary

**FinSight** is an end-to-end interactive finance analytics dashboard built in Power BI. It connects two raw CSV data sources — transaction records and customer profiles — through a star schema data model, enabling dynamic filtering and real-time year-over-year comparisons across every metric.

### Key capabilities:
- **Dynamic Metric Switching** — toggle between Total Amount, Total Fee, Total Tax, and Avg Transaction Value using a single field parameter
- **Year-over-Year KPIs** — all five headline metrics show % change vs the prior year
- **Multi-dimension Slicers** — filter by Year, Occupation, Merchant Category, and Customer Segment simultaneously
- **Geo Analysis** — ranked state-level bar chart for any selected metric
- **Transaction Type Matrix** — heatmap table showing Amount, Fee, Tax, and Count per transaction type
- **Drillthrough Page** — row-level transaction detail with all 2024 records

---

## 📈 Key Metrics (2024 — Business Owner · Bank Charges)

| Metric | Value | vs Previous Year |
|--------|-------|-----------------|
| Total Amount | $1.67M | ↓ 5.09% |
| Total Transactions | 170 | ↓ 5.56% |
| Avg Transaction Value | $9.84K | ↑ 0.49% |
| Total Fee | $2.55K | ↓ 9.17% |
| Total Tax | $458.73 | ↓ 9.17% |

---

## 💡 Business Insights

- **Karnataka leads** all states in total fees at $559.89, followed by Gujarat ($468.11) and Tamil Nadu ($352.74)
- **80.9% success rate** on transactions; failed transactions are just 2.8% — a healthy payment infrastructure signal
- **Retail segment** contributes $1.4K in fees — nearly 3× the SME segment — the highest-value customer cohort
- **Loan EMI** is the top transaction type at $0.7M in total amount
- **Near gender parity**: Female customers account for 51.4% of total fees vs 48.6% male
- **YoY decline alert**: Amount, count, and fees all down 5–9% — warrants retention-focused cohort analysis

---

## 🗄️ Data Model

Star schema with two source tables joined on `customer_id`, plus a generated `DateTable` for time intelligence.

```
DateTable (DIM)
    │
    └── transaction_date
            │
finance_transactions (FACT · 50,069 rows) ──── customer_id ──── customers (DIM · 5,000 rows)
```

### `finance_transactions` — Fact Table

| Column | Type | Description |
|--------|------|-------------|
| `transaction_id` 🔑 | TEXT | Unique transaction identifier |
| `transaction_date` | DATE | Date of transaction |
| `account_id` | TEXT | Account reference |
| `customer_id` 🔗 | TEXT | Foreign key → customers |
| `transaction_type` | TEXT | Loan EMI, Deposit, Withdrawal, etc. |
| `channel` | TEXT | Online, ATM, Branch, etc. |
| `merchant_category` | TEXT | Bank Charges, Shopping, etc. |
| `amount` | DECIMAL | Transaction amount in USD |
| `fee_amount` | DECIMAL | Fee charged |
| `tax_amount` | DECIMAL | Tax applied |
| `currency` | TEXT | Currency code |
| `transaction_status` | TEXT | Success / Pending / Failed |
| `is_fraud` | BOOL | Fraud flag |
| `risk_score` | INT | 0–100 risk score |
| `reference_no` | TEXT | Reference number |

### `customers` — Dimension Table

| Column | Type | Description |
|--------|------|-------------|
| `customer_id` 🔑 | TEXT | Unique customer ID |
| `first_name` | TEXT | First name |
| `second_name` | TEXT | Last name |
| `gender` | TEXT | Male / Female |
| `date_of_birth` | DATE | Date of birth |
| `city` | TEXT | City |
| `state` | TEXT | Indian state |
| `occupation` | TEXT | Business Owner, Salaried, etc. |
| `customer_segment` | TEXT | Retail / SME / Corporate / Premium / Wealth |
| `annual_income` | DECIMAL | Annual income in USD |
| `join_date` | DATE | Customer onboarding date |

---

## ⚡ Key DAX Measures

```dax
-- Core KPIs
Total Amount          = SUM( finance_transactions[amount] )
Total Fee             = SUM( finance_transactions[fee_amount] )
Total Tax             = SUM( finance_transactions[tax_amount] )
Avg Transaction Value = AVERAGE( finance_transactions[amount] )
Total Transaction     = COUNTROWS( finance_transactions )

-- Year-over-Year
PY Total Amount =
CALCULATE(
    [Total Amount],
    SAMEPERIODLASTYEAR( DateTable[Date] )
)

YoY % Amount =
DIVIDE(
    [Total Amount] - [PY Total Amount],
    [PY Total Amount],
    0
)

-- Date Table
DateTable =
ADDCOLUMNS(
    CALENDAR( DATE(2020,1,1), DATE(2025,12,31) ),
    "Year",    YEAR([Date]),
    "Month",   FORMAT([Date], "MMM"),
    "MonthNo", MONTH([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "WeekDay", FORMAT([Date], "dddd")
)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Power BI Desktop | Report authoring and visualisation |
| DAX | Calculated measures and KPI logic |
| Power Query (M) | Data cleaning and transformation |
| Field Parameters | Dynamic metric switching |
| CSV Files | Source data (transactions + customers) |

---

## 📁 Repository Structure

```
finsight-finance-dashboard/
├── data/
│   ├── finance_transactions.csv    # 50,069 transaction records
│   └── customers.csv               # 5,000 customer profiles
├── screenshots/
│   ├── overview_analysis.png       # Overview page
│   └── transactions_detail.png     # Drillthrough page
├── FInsightDataanalysis.pbix       # Main Power BI report
└── README.md
```

---

## 🚀 Getting Started

**1. Clone the repo**
```bash
git clone https://github.com/YOUR_USERNAME/finsight-finance-dashboard.git
cd finsight-finance-dashboard
```

**2. Install Power BI Desktop**  
Download free at [powerbi.microsoft.com/desktop](https://powerbi.microsoft.com/en-us/desktop/) (Windows).  
Mac users: publish to Power BI Service and view at `app.powerbi.com`.

**3. Open the report**  
Open `FInsightDataanalysis.pbix` in Power BI Desktop. All data is embedded — no external connection required.

**4. (Optional) Refresh with updated data**  
Go to `Home → Transform Data → Data Source Settings` and point to your local `data/` folder.

**5. Explore**  
Use the Year, Dynamic Metric, Occupation, and Category slicers in the left panel to filter all visuals dynamically.

---

## 📋 Dashboard Pages

| Page | Description |
|------|-------------|
| **Overview Analysis** | KPI cards, monthly trend line, state geo bar, transaction status donut, customer segment bars, gender donut, transaction type matrix |
| **Transactions** | Full drillthrough table with all individual transaction records, sortable by any column |

---

## 🔮 Future Enhancements

- [ ] Add fraud detection visual (is_fraud flag analysis)
- [ ] Risk score distribution histogram
- [ ] Customer lifetime value (CLV) calculation
- [ ] Mobile-optimised layout
- [ ] Azure SQL / SharePoint live connection

---

## 📄 License

MIT License — free to use, fork, and build upon with attribution.

---

## 🙌 Connect

If this project helped you, please give it a ⭐ on GitHub!

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com)
