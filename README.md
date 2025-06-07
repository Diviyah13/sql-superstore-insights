
# 📊 SQL Project: Business Insights from Superstore Sales Data

##  Project Goal
Used SQL to analyze customer behavior, product sales, and revenue growth from a Superstore dataset. This project simulates real business problems and shows how SQL can be used for decision-making.

---

## 🔍 Insight 1: Who are the most valuable and loyal customers?

```sql
WITH customer_orders AS (
  SELECT 
    Customer_ID,
    COUNT(DISTINCT Order_ID) AS total_orders,
    SUM(Sales) AS total_sales
  FROM superstore
  GROUP BY Customer_ID
),
classified_customers AS (
  SELECT *,
    CASE 
      WHEN total_orders > 1 THEN 'Repeat'
      ELSE 'One-time'
    END AS customer_type
  FROM customer_orders
)
SELECT 
  customer_type,
  COUNT(*) AS num_customers,
  ROUND(AVG(total_sales), 2) AS avg_sales_per_customer
FROM classified_customers
GROUP BY customer_type;
```

**Insight:** Repeat customers tend to generate higher average sales. You could implement loyalty rewards based on this.

---

##  Insight 2: Is our monthly revenue growing?

```sql
SELECT 
  EXTRACT(YEAR FROM Order_Date) AS year,
  EXTRACT(MONTH FROM Order_Date) AS month,
  ROUND(SUM(Sales), 2) AS total_sales,
  ROUND(SUM(Sales) - LAG(SUM(Sales)) OVER (ORDER BY EXTRACT(YEAR FROM Order_Date), EXTRACT(MONTH FROM Order_Date)), 2) AS sales_change
FROM superstore
GROUP BY year, month
ORDER BY year, month;
```

**Insight:** Track monthly revenue trends to guide promotional strategies and seasonal marketing.

---

##  Insight 3: Are we seeing customer churn?

```sql
WITH last_order AS (
  SELECT 
    Customer_ID,
    MAX(Order_Date) AS last_order_date
  FROM superstore
  GROUP BY Customer_ID
)
SELECT 
  Customer_ID,
  last_order_date,
  CURRENT_DATE - last_order_date AS days_since_last_order
FROM last_order
ORDER BY days_since_last_order DESC;
```

**Insight:** Follow up with customers who haven’t ordered in over 180 days to reduce churn.

---

##  Tools Used
- SQL (PostgreSQL-style syntax)
- Dataset: Superstore Sales Data

## 📬 Contact
**Author**: Diviyah  
Let’s connect on [LinkedIn](https://www.linkedin.com) and grow together in data!  
