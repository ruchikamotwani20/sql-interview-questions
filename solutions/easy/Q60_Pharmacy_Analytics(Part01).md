# Q60 – Pharmacy Analytics (Part 01)  
**Source:** CVS Health  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table `pharmacy_sales` containing drug sales data with:

- `drug`
- `total_sales`
- `cogs` (cost of goods sold)

Your task is to:  
> Find the **top 3 most profitable drugs**, where profit is calculated as:  
**`total_sales - cogs`**

---

## 🧾 Table – `pharmacy_sales`

| drug         | total_sales | cogs   |
|--------------|-------------|--------|
| Aspirin      | 50000       | 30000  |
| Ibuprofen    | 60000       | 25000  |

---

## 🔍 Solution – Calculate Profit and Sort

```sql
SELECT 
  drug, 
  (total_sales - cogs) AS total_proft
FROM pharmacy_sales
ORDER BY (total_sales - cogs) DESC
LIMIT 3;
```

💡 Explanation

total_sales - cogs gives the profit.

We order by profit in descending order.

Use LIMIT 3 to get the top 3 most profitable drugs.
