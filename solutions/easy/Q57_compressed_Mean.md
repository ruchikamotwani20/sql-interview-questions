# Q57 – Compressed Mean  
**Source:** Alibaba  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a compressed table called `items_per_order`, where:

- `item_count`: number of items in an order
- `order_occurrences`: how many orders had exactly that many items

Your task is to:  
> Compute the **mean number of items per order**, based on this compressed table.  
Round the final result to **1 decimal place**.

---

## 🧾 Table – `items_per_order`

| item_count | order_occurrences |
|------------|-------------------|
| 1          | 10                |
| 2          | 20                |
| 3          | 15                |

---

## 🔍 Solution – Weighted Average with Numeric Cast

```sql
SELECT 
  ROUND(
    SUM(item_count * order_occurrences)::NUMERIC / SUM(order_occurrences), 
    1
  ) AS mean
FROM items_per_order;
```
💡 Explanation

This query calculates a weighted average, multiplying the number of items by the number of times it occurred.

::NUMERIC is used to cast the result from SUM(...) to a format PostgreSQL allows to round to decimals.

ROUND(..., 1) ensures 1-digit precision.


