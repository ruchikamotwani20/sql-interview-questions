# Q5_Highest_Grossing_Items
**Source:** Amazon  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a `product_spend` table with:
- `category`
- `product`
- `user_id`
- `spend`
- `transaction_date`

Your task is to:
- Find the **top 2 products** with the **highest total spend** in each category
- Only consider transactions from the year **2022**

---

## 🔍 Solution – Using `ROW_NUMBER()`

```sql
SELECT category, product, total_spend
FROM (
  SELECT 
    category, 
    product, 
    SUM(spend) AS total_spend,
    ROW_NUMBER() OVER (
      PARTITION BY category 
      ORDER BY SUM(spend) DESC
    ) AS rnk
  FROM product_spend
  WHERE EXTRACT(YEAR FROM transaction_date) = 2022
  GROUP BY category, product
) sub
WHERE rnk <= 2
ORDER BY category, total_spend DESC;
```

💡 Explanation

We filter for transactions that occurred in 2022 using EXTRACT(YEAR FROM transaction_date) = 2022

Group by category and product, and calculate SUM(spend) to get total spend per product

Use ROW_NUMBER() partitioned by category to rank products by spend

Filter where rnk <= 2 to keep the top 2 products per category

Final output is sorted by category and descending spend

