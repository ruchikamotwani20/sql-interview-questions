# Q20 – YoY Growth Rate  
**Source:** Wayfair  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given a table:

- `user_transactions`: contains historical spend data per product over time.

Your task is to:  
> For each product, calculate the **year-on-year (YoY) growth rate** of total spend.  
> Output the year, product_id, current year’s spend, previous year’s spend, and growth percentage rounded to 2 decimal places.

---

## 🧾 Table

### `user_transactions`

| transaction_id | product_id | spend   | transaction_date |
|----------------|------------|---------|------------------|
| 1341           | 123424     | 1500.60 | 2019-12-31       |
| 1423           | 123424     | 1000.20 | 2020-12-31       |
| 1623           | 123424     | 1246.44 | 2021-12-31       |
| 1322           | 123424     | 2145.32 | 2022-12-31       |

---

## 🔍 Solution – Using CTE and LAG

```sql
WITH cte AS (
  SELECT 
    EXTRACT(YEAR FROM transaction_date) AS year,
    product_id,
    spend AS curr_year_spend,
    LAG(spend) OVER (PARTITION BY product_id ORDER BY transaction_date) AS prev_year_spend
  FROM user_transactions
)

SELECT *,
  ROUND(
    100.0 * (curr_year_spend - prev_year_spend) / prev_year_spend,
    2
  ) AS yoy_rate
FROM cte;
```

💡 Explanation

1️⃣ Use a CTE to extract the year, product_id, and current spend

2️⃣ Use LAG() to get previous year’s spend per product

3️⃣ In the final SELECT, calculate:

YoY = (current−previous)/previous × 100

4️⃣ Use ROUND(..., 2) to get growth rate to 2 decimal places
