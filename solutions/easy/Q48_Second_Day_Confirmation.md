# Q48 – Second Day Confirmation  
**Source:** TikTok  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given two tables:

- `emails`: contains `user_id`, `email_id`, `signup_date`, and `signup_action`
- `texts`: contains `email_id`, `action_date`

Your task is to:  
> Return the `user_id` of users who **confirmed** their signup exactly **one day after** they signed up.

---

## 🧾 Tables  

### `emails`  
| user_id | email_id | signup_date | signup_action |
|---------|----------|--------------|----------------|
| 123     | 999      | 2022-01-01   | Confirmed      |

### `texts`  
| email_id | action_date |
|----------|--------------|
| 999      | 2022-01-02   |

---

## 🔍 Solution – Date Difference + Filter

```sql
SELECT user_id
FROM emails e
JOIN texts t USING(email_id)
WHERE signup_action = 'Confirmed'
  AND EXTRACT(DAY FROM (action_date - signup_date)) = 1;
```
💡 Explanation

Join the emails and texts tables using email_id.

Filter for users whose signup_action is 'Confirmed'.

Compute the difference between action_date and signup_date, and select those where the difference is exactly 1 day.
