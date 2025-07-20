# Q63 – Pharmacy Analytics (Part 02)  
**Source:** CVS Health  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given the `pharmacy_sales` table that contains sales and cost data for various drugs.

Your task is to:  
> For each **manufacturer**, find out how many drugs they lost money on (i.e., `total_sales < cogs`)  
> Also calculate the **total loss** per manufacturer, defined as:  
**`cogs - total_sales`** (only for those where sales were less than cost)

---

## 🧾 Table – `pharmacy_sales`

| manufacturer     | drug_name  | total_sales | cogs |
|------------------|------------|-------------|------|
| HealthCorp       | MedA       | 4500        | 5000 |
| Pharma Inc.      | MedB       | 3000        | 4000 |

---

## 🔍 Solution – Conditional Aggregation

```sql
SELECT
    manufacturer,
    COUNT(*) AS drug_count,
    ROUND(SUM(cogs - total_sales), 2) AS total_loss
FROM
    pharmacy_sales
WHERE
    total_sales < cogs
GROUP BY
    manufacturer
ORDER BY
    total_loss DESC;
```

💡 Explanation

The WHERE clause filters only loss-making drugs.

COUNT(*) counts how many drugs a manufacturer lost money on.

SUM(cogs - total_sales) computes total monetary loss.

ROUND(..., 2) formats the result to 2 decimal places.

Finally, we sort by the highest total loss.
