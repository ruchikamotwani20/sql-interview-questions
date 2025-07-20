# Q66 – Pharmacy Health (Part 03)  
**Source:** CVS Health  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a `pharmacy_sales` table containing sales data for drugs and manufacturers.

Your task is to:  
> For each **manufacturer**, return the total sales **formatted in millions** (e.g., `$45 million`), sorted by total sales descending and manufacturer name ascending.

---

## 🧾 Table – `pharmacy_sales`

| manufacturer     | drug_name  | total_sales |
|------------------|------------|-------------|
| PharmaCorp       | MedA       | 50000000    |
| HealthMed Inc.   | MedB       | 20000000    |

---

## 🔍 Solution – Aggregate and Format in Millions

```sql
SELECT 
  manufacturer,
  CONCAT('$', ROUND(SUM(total_sales) / 1000000), ' million') AS sale 
FROM pharmacy_sales
GROUP BY manufacturer
ORDER BY SUM(total_sales) DESC, manufacturer ASC;
```
💡 Explanation

- SUM(total_sales) computes total sales for each manufacturer.

- Divide by 1,000,000 and wrap with ROUND() to get a clean million-format.

- CONCAT() adds the dollar sign and "million" label.

- Sort by total sales (desc) and name (asc) to meet business formatting needs.
