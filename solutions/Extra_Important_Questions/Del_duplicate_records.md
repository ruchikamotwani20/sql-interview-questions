# Removing Duplicate Orders While Keeping the Lowest `order_id`

This guide shows how to **delete duplicate rows** from an `Orders` table while **keeping only the row with the lowest `order_id`** for each group of duplicates.

---

## Table Structure
**Table Name:** `Orders`  
**Columns:**  
- `order_id` (Primary key, unique order identifier)  
- `customer_id`  
- `order_date`  
- `amount`  

---

## Example Data

```sql
order_id | customer_id | order_date  | amount
1        | 101         | 2025-07-01  | 100
2        | 101         | 2025-07-01  | 100
3        | 102         | 2025-07-02  | 200
4        | 101         | 2025-07-01  | 100
5        | 103         | 2025-07-03  | 300
```

Goal:

Keep only the row with the smallest order_id for each duplicate group (customer_id + order_date + amount).

Delete all others.

---

## Solution 1: Using a CTE with ROW_NUMBER()
```sql
WITH cte AS (
    SELECT 
        order_id,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id, order_date, amount 
            ORDER BY order_id
        ) AS rn
    FROM orders
)
DELETE FROM orders
WHERE order_id IN (
    SELECT order_id FROM cte WHERE rn > 1
);
```

## How it works:

- ROW_NUMBER(): Assigns a row number to each record within a duplicate group (PARTITION BY), ordering by order_id so the smallest gets 1.

- cte: Holds these row numbers for all rows.

- DELETE: Removes all rows where rn > 1 (i.e., all but the first/lowest order_id in each group).

---

## Solution 2: Simple Query with MIN() and NOT IN
``` sql
DELETE FROM orders
WHERE order_id NOT IN (
    SELECT MIN(order_id)
    FROM orders
    GROUP BY customer_id, order_date, amount
);
```




