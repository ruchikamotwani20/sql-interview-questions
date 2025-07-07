# Q22 – Third Transaction Per User  
**Source:** Uber  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a `transactions` table with columns:
- `user_id`
- `spend`
- `transaction_date`

Your task is to find **each user's third transaction** (chronologically) and return:
- `user_id`
- `spend`
- `transaction_date`

---

## 🔄 Solution – Using `ROW_NUMBER()` and CTE

```sql
WITH ranked_txns AS (
  SELECT 
    user_id, 
    spend, 
    transaction_date,
    ROW_NUMBER() OVER (
      PARTITION BY user_id 
      ORDER BY transaction_date
    ) AS rn
  FROM transactions
)

SELECT 
  user_id, 
  spend, 
  transaction_date
FROM ranked_txns
WHERE rn = 3;
```

💡 Explanation

The ROW_NUMBER() function assigns a row number to each transaction within each user, ordered by date.

The CTE (ranked_txns) stores this result.

We then select only the rows where rn = 3, i.e., the third transaction for each user.
