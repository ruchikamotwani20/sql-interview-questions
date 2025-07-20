# Q59 – Repeated Payments  
**Source:** Stripe  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given a `transactions` table that records payment details including:

- `transaction_id`
- `merchant_id`
- `credit_card_id`
- `amount`
- `transaction_timestamp`

Your task is to:  
> Identify how many transactions appear to be **accidental duplicate payments**, defined as:  
Two transactions that:
- Have the **same `merchant_id`, `credit_card_id`, and `amount`**
- Occur within **10 minutes** of each other

Count only the **later (duplicate) transaction**.

---

## 🧾 Table – `transactions`

| transaction_id | merchant_id | credit_card_id | amount | transaction_timestamp     |
|----------------|-------------|----------------|--------|----------------------------|
| 1              | 10          | 123            | 100.00 | 2022-01-01 10:00:00        |
| 2              | 10          | 123            | 100.00 | 2022-01-01 10:05:00        |

---

## 🔍 Solution – Self-Join with Time Comparison

```sql
SELECT COUNT(DISTINCT t2.transaction_id) AS payment_count
FROM transactions t1
JOIN transactions t2
  ON t1.merchant_id = t2.merchant_id
  AND t1.credit_card_id = t2.credit_card_id
  AND t1.amount = t2.amount
  AND t2.transaction_timestamp > t1.transaction_timestamp
  AND t2.transaction_timestamp <= t1.transaction_timestamp + INTERVAL '10 minutes'
```
💡 Explanation

- We self-join the table where each transaction (t2) is compared to a previous one (t1) with the same merchant, credit card, and amount.

- The time condition ensures we only consider duplicates that occurred within 10 minutes of the original.

- We COUNT(DISTINCT t2.transaction_id) to avoid duplicate counting and only count the later payment.
