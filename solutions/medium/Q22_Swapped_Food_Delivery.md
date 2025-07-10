# Q22 – Swapped Food Delivery  
**Source:** Zomato  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table:

- `orders`: contains `order_id` and the `item` delivered.

Due to a system error, each item's order was **swapped with the next one**.  
Your task is to:  
> Fix the swapped order IDs and return the **correct pairing** of `order_id` and `item`.  
> If the last order has an **odd order_id**, keep it unchanged.

---

## 🧾 Table

### `orders`

| order_id | item             |
|----------|------------------|
| 1        | Chow Mein        |
| 2        | Pizza            |
| 3        | Pad Thai         |
| 4        | Butter Chicken   |
| 5        | Eggrolls         |
| 6        | Burger           |
| 7        | Tandoori Chicken |

---

## 🔍 Solution – Using CASE and Swapping Logic

```sql
SELECT
  CASE
    WHEN (order_id = (SELECT MAX(order_id) FROM orders)) AND order_id % 2 != 0 THEN order_id
    WHEN order_id % 2 = 0 THEN order_id - 1
    ELSE order_id + 1
  END AS corrected_order_id,
  item
FROM orders
ORDER BY corrected_order_id;
```
💡 Explanation

1️⃣ order_id % 2 != 0 means it's an odd number (like 1, 3, 5, etc.)

2️⃣ If it’s the last row and odd, don’t change it

3️⃣ For other odd numbers, swap with the next order (order_id + 1)

4️⃣ For even numbers, swap with the previous order (order_id - 1)

5️⃣ ORDER BY corrected_order_id gives the final fixed sequence

