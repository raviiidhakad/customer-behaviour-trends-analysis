# customer-behaviour-trends-analysis
# 🛍️ Customer Shopping Behavior Analysis

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-SQL-336791?logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📌 Project Overview

A comprehensive end-to-end data analytics project analyzing **3,900 customer transactions** across a retail company. The project uncovers actionable insights into spending patterns, customer segments, product preferences, and subscription behavior using **Python**, **SQL (PostgreSQL)**, and **Power BI**.

> **Business Question:** *"How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?"*

---

## 📁 Repository Structure

```
customer-shopping-behavior-analysis/
│
├── 📂 data/
│   └── customer_shopping_behavior.csv          # Raw dataset (3,900 rows, 18 columns)
│
├── 📂 notebooks/
│   └── Customer_Shopping_Behavior_Analysis.ipynb  # EDA & data cleaning in Python
│
├── 📂 sql/
│   └── customer_behavior_sql_queries.sql       # 10 business SQL queries (PostgreSQL)
│
├── 📂 dashboard/
│   └── customer_behavior_dashboard.pbix        # Interactive Power BI dashboard
│
├── 📂 reports/
│   ├── Customer_Shopping_Behavior_Analysis.pdf # Full project report
│   └── Customer-Shopping-Behavior-Analysis.pptx  # Stakeholder presentation
│
└── README.md
```

---

## 📊 Dataset Overview

| Feature | Details |
|---|---|
| **Rows** | 3,900 transactions |
| **Columns** | 18 features |
| **Demographics** | Age, Gender, Location, Subscription Status |
| **Purchase Info** | Item, Category, Amount (USD), Season, Size, Color |
| **Behavior** | Discount Applied, Previous Purchases, Review Rating, Shipping Type |
| **Missing Data** | 37 values in `Review Rating` — imputed using category median |

---

## 🔧 Tools & Technologies

| Layer | Tool |
|---|---|
| **Data Cleaning & EDA** | Python (Pandas, NumPy, Matplotlib, Seaborn) |
| **Database & Querying** | PostgreSQL (via SQLAlchemy) |
| **Visualization** | Power BI |
| **Reporting** | PDF Report + PowerPoint Presentation |

---

## 🐍 Python — Data Preparation

Key steps performed in the Jupyter Notebook:

- **Data Loading** — Imported dataset using `pandas`
- **Missing Value Imputation** — Filled 37 missing `review_rating` values with category-level medians
- **Column Standardization** — Renamed all columns to `snake_case`
- **Feature Engineering**:
  - Created `age_group` by binning customer ages (Young Adult / Adult / Middle-aged / Senior)
  - Created `purchase_frequency_days` from purchase frequency data
- **Redundancy Check** — Verified `discount_applied` and `promo_code_used` were duplicates; dropped `promo_code_used`
- **Database Integration** — Exported cleaned DataFrame to PostgreSQL via SQLAlchemy

---

## 🗄️ SQL — Business Queries (PostgreSQL)

10 analytical queries were written to answer core business questions:

| # | Business Question | Technique |
|---|---|---|
| Q1 | Revenue by Gender | `GROUP BY`, `SUM` |
| Q2 | High-Spending Discount Users | Subquery, `WHERE` filter |
| Q3 | Top 5 Products by Review Rating | `AVG`, `ORDER BY`, `LIMIT` |
| Q4 | Standard vs. Express Shipping Spend | Conditional `GROUP BY` |
| Q5 | Subscribers vs. Non-Subscribers Revenue | `COUNT`, `AVG`, `SUM` |
| Q6 | Most Discount-Dependent Products | `CASE WHEN`, percentage calculation |
| Q7 | Customer Segmentation (New / Returning / Loyal) | `CTE`, `CASE WHEN` |
| Q8 | Top 3 Products per Category | `ROW_NUMBER()`, Window Function |
| Q9 | Repeat Buyers & Subscription Likelihood | Filtering + `GROUP BY` |
| Q10 | Revenue by Age Group | Grouped aggregation |

### Sample Query — Customer Segmentation (CTE + CASE WHEN)
```sql
WITH customer_type AS (
  SELECT customer_id, previous_purchases,
    CASE 
      WHEN previous_purchases = 1 THEN 'New'
      WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
      ELSE 'Loyal'
    END AS customer_segment
  FROM customer
)
SELECT customer_segment, COUNT(*) AS "Number of Customers"
FROM customer_type
GROUP BY customer_segment;
```

---

## 📈 Key Findings

| Insight | Finding |
|---|---|
| 💰 **Revenue by Gender** | Male customers generated **2× more revenue** ($157,890) vs. female ($75,191) |
| 🧾 **Discount Users** | 839 customers used discounts yet spent **above average** — high-value targets |
| ⭐ **Top Rated Product** | Gloves (3.86), Sandals (3.84), Boots (3.82) |
| 🚚 **Shipping & Spend** | Express shipping users spend slightly more ($60.48 vs $58.46 Standard) |
| 🔄 **Subscriptions** | Only 27% are subscribers; avg spend is nearly equal (~$59.49 vs $59.87) |
| 👥 **Customer Segments** | 3,116 Loyal / 701 Returning / 83 New customers |
| 🏷️ **Discount-Heavy Items** | Hat (50%), Sneakers (49.66%), Coat (49.07%) — review margin impact |
| 📦 **Top Category** | Clothing leads in both revenue and sales volume |
| 🔁 **Repeat Buyers** | Most repeat buyers (>5 purchases) are **not** subscribed (2,518 vs 958) |
| 🎂 **Revenue by Age** | Young Adults lead ($62,143), followed closely by Middle-aged ($59,197) |

---

## 📊 Power BI Dashboard

The interactive dashboard includes:

- **KPI Cards** — Total Customers (3.9K), Avg Purchase Amount ($59.76), Avg Rating (3.75)
- **Donut Chart** — Subscription status split (Yes 27% / No 73%)
- **Bar Charts** — Revenue & Sales by Category and Age Group
- **Slicers** — Filter by Gender, Category, Subscription Status, Shipping Type

---

## 💡 Business Recommendations

1. **Boost Subscriptions** — Promote exclusive perks to convert the 73% non-subscribers, especially repeat buyers who haven't subscribed
2. **Loyalty Programs** — Reward returning customers to accelerate their move into the Loyal segment
3. **Revise Discount Strategy** — Hat, Sneakers, and Coat have ~50% discount rates; reassess margin impact
4. **Promote Top-Rated Products** — Feature Gloves, Sandals, and Boots in marketing campaigns
5. **Target Young Adults** — Highest revenue-contributing segment; prioritize in ad spend
6. **Express Shipping Upsell** — Express users spend more; promote it as a premium option

---

## 🚀 How to Run

### Python Notebook
```bash
pip install pandas numpy matplotlib seaborn sqlalchemy psycopg2
jupyter notebook notebooks/Customer_Shopping_Behavior_Analysis.ipynb
```

### SQL Queries
```bash
# Connect to PostgreSQL and run:
psql -U your_user -d your_database -f sql/customer_behavior_sql_queries.sql
```

### Power BI Dashboard
Open `dashboard/customer_behavior_dashboard.pbix` in **Power BI Desktop**.

---

## 👤 Author

**Ravi Dhakad**  
📧 raviiidhakad@gmail.com  
🔗 [LinkedIn] https://www.linkedin.com/in/ravi-dhakad-data-analyst/

---

> ⭐ If you found this project useful, consider starring the repository!

