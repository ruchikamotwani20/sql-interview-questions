# Q8 – TikTok Signup Activation Rate  
**Source:** TikTok  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given two tables:

- `emails(email_id, user_id, signup_date)`
- `texts(email_id, signup_action, action_date)`

The `texts` table logs if a user **confirmed** their signup via SMS (`signup_action = 'Confirmed'`).

Your task is to calculate the **activation rate**, defined as:

> Percentage of users who confirmed their signup  
> = (Confirmed users) / (Total users) — rounded to 2 decimal places

---

## 🔍 Solution

```sql
SELECT 
  ROUND(
    COUNT(DISTINCT CASE WHEN t.signup_action = 'Confirmed' THEN e.email_id END) * 1.0
    / COUNT(DISTINCT e.email_id),
    2
  ) AS confirm_rate
FROM emails e
LEFT JOIN texts t
  ON e.email_id = t.email_id;
```

💡 Explanation

COUNT(DISTINCT e.email_id): total number of users who signed up via email

CASE WHEN ... THEN END: filters only users with 'Confirmed' actions

LEFT JOIN: ensures users with no confirmation still appear in the denominator

Multiply by 1.0 to avoid integer division

ROUND(..., 2): round the result to two decimal places


