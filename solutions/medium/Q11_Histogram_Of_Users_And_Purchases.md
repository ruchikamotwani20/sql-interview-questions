# Q11 – Number of Products on Most Recent Transaction Date  
**Source:** Walmart  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a `user_transactions` table with:

- `product_id`
- `user_id`
- `spend`
- `transaction_date`

Your task is to:
> For each user, return their most recent transaction date along with the number of products they purchased that day.

---

## 🔍 Solution – Using Subquery with `(user_id, MAX(transaction_date))`

```sql
SELECT 
  transaction_date, 
  user_id, 
  COUNT(*) AS purchase_count
FROM user_transactions
WHERE (user_id, transaction_date) IN (
  SELECT 
    user_id, 
    MAX(transaction_date)
  FROM user_transactions
  GROUP BY user_id
)
GROUP BY transaction_date, user_id
ORDER BY transaction_date;
```

💡 Explanation

The subquery finds each user's most recent transaction_date.

The main query filters only those rows.

Then we group by user_id and transaction_date to count how many products they purchased on that day.
