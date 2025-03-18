# Retail_Order_Data_Processing
This project combines Python and SQL to analyze retail order data and extract key business insights. The goal is to understand sales performance, top-selling products, and customer behavior across different regions and time periods.

---

## Objective

To perform end-to-end data analysis on retail orders and generate business intelligence reports using both Python (Pandas/Matplotlib) and SQL queries. The analysis reveals product performance, seasonal trends, and regional sales insights.

---

##  Tools & Technologies

- **Python**: Data processing & visualization
- **Jupyter Notebook**: Interactive data analysis
- **SQL**: Business intelligence queries
- **Pandas, Seaborn, Matplotlib**: Data wrangling and visualization
- **VSCode / JupyterLab**: For local development
- **GitHub**: For version control and showcasing the project

---


##  Python Analysis Highlights

 **File**: `Retail Order Data Processing.ipynb`

###  Key Steps:

1. **Data Loading**  
   Load the retail order data using pandas.

2. **Data Cleaning**  
   - Handle missing values
   - Convert data types
   - Parse dates

3. **Data Exploration**  
   - Total sales
   - Average order value
   - Number of unique products


---

## 🧾 SQL Business Insight Queries

 **File**: `sql_code.sql`

This file contains SQL queries that answer real business questions:

###  1. Top 10 Revenue-Generating Products

```sql
SELECT TOP 10 product_id, SUM(sale_price) AS sales
FROM df_orders
GROUP BY product_id
ORDER BY sales DESC;
```

###  2. Top 5 Best-Selling Products by Region

```sql
WITH cte AS (
    SELECT region, product_id, SUM(sale_price) AS sales
    FROM df_orders
    GROUP BY region, product_id
)
SELECT *
FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY region ORDER BY sales DESC) AS rn
    FROM cte
) A
WHERE rn <= 5;
```

###  3. Month-over-Month Sales Growth (2022 vs 2023)

```sql
WITH cte AS (
    SELECT YEAR(order_date) AS order_year, MONTH(order_date) AS order_month,
           SUM(sale_price) AS sales
    FROM df_orders
    GROUP BY YEAR(order_date), MONTH(order_date)
)
SELECT order_month,
       SUM(CASE WHEN order_year = 2022 THEN sales ELSE 0 END) AS sales_2022,
       SUM(CASE WHEN order_year = 2023 THEN sales ELSE 0 END) AS sales_2023
FROM cte
GROUP BY order_month;
```

---

##  Insights & Conclusions

-  **Concentration of Sales**: A small set of products contributes to the majority of revenue.
-  **Seasonal Trends**: Sales peak during certain months, which helps in planning marketing campaigns.
-  **Regional Preferences**: Different regions prefer different product types – useful for inventory planning.
-  **Year-over-Year Comparison**: 2023 saw a growth in sales compared to 2022 in most months.

---
