# Q12 – Mode of Item Counts in Orders  
**Source:** Alibaba  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table `items_per_order` with:

- `item_count`: number of items in an order  
- `order_occurrences`: how many times this item count occurred

Your task is to:
> Return the item count(s) that occurred most frequently (i.e., the **mode**).  
> If multiple item counts tie for highest frequency, return all of them — sorted in ascending order.

---

## 🔍 Solution – Using Subquery and `MAX`

```sql
SELECT item_count AS mode
FROM (
  SELECT item_count 
  FROM items_per_order 
  WHERE order_occurrences = (
    SELECT MAX(order_occurrences) 
    FROM items_per_order
  )
) AS sub
ORDER BY item_count;
```

💡 Explanation

Inner subquery finds the maximum occurrence count from the table.

The WHERE clause filters only item_counts that match this max frequency.

If there's a tie, multiple rows will be returned.

Final ORDER BY item_count ensures output is sorted as required.
