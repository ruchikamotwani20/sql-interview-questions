# Q43 – User Shopping Sprees  
**Source:** Amazon  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table `transactions` with:

- `user_id`
- `transaction_date`

Your task is to:  
> Find all users who made **3 purchases on 3 consecutive days**.

Return a list of such `user_id`s in ascending order.

---

## 🧾 Table  

### `transactions`  
| user_id | transaction_date      |
|---------|------------------------|
| 1       | 2022-01-01 00:00:00    |
| 1       | 2022-01-02 00:00:00    |
| 1       | 2022-01-03 00:00:00    |
| 2       | 2022-01-01 00:00:00    |
| 2       | 2022-01-03 00:00:00    |

---

## 🔍 Solution – Self Join with Date Offset Logic

```sql
SELECT t1.user_id 
FROM transactions t1
INNER JOIN transactions t2
  ON t1.user_id = t2.user_id
  AND DATE(t1.transaction_date) = DATE(t2.transaction_date) + 1
INNER JOIN transactions t3
  ON t1.user_id = t3.user_id
  AND DATE(t1.transaction_date) = DATE(t3.transaction_date) + 2
ORDER BY t1.user_id;
```
💡 Explanation

We're trying to find 3 consecutive-day purchases.

We use self-joins on the same table:

t1 is the transaction on Day 3

t2 is on Day 2 (1 day before)

t3 is on Day 1 (2 days before)
