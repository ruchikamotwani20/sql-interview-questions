# Q17 – Active User Retention  
**Source:** Facebook

**Difficulty:** Hard  

---

## ✅ Goal  
You're given a table:

- `user_actions`: contains user events like sign-ins, comments, and likes.

Your task is to:  
> Calculate the number of users who were active in **both June and July 2022**, where "active" means performing at least one `sign-in`, `comment`, or `like`.  
> Return the result as `monthly_active_users` for **month = 7** (i.e., July).

---

## 🧾 Table

### `user_actions`

| user_id | event_type | event_date |
|---------|------------|------------|
| 1001    | sign-in    | 2022-06-14 |
| 1001    | like       | 2022-07-02 |
| 1002    | comment    | 2022-07-15 |
| 1003    | sign-in    | 2022-06-11 |

---

## 🔍 Solution – Using GROUP BY and LAG

```sql
SELECT 7 AS month, COUNT(*) AS monthly_active_users
FROM (
  SELECT user_id,
         COUNT(DISTINCT EXTRACT(MONTH FROM event_date)) AS month
  FROM user_actions
  WHERE event_type IN ('sign-in', 'comment', 'like')
    AND event_date BETWEEN '2022-06-01' AND '2022-07-31'
  GROUP BY user_id
) AS active_users
WHERE month = 2;
```
💡 Explanation

1️⃣ Filter only sign-in, comment, and like events
✅ These are considered user activity

2️⃣ Restrict to events between 2022-06-01 and 2022-07-31
✅ We're only interested in June and July

3️⃣ Group by user_id and count distinct months
✅ This gives how many of those 2 months each user was active

4️⃣ Keep users where month = 2
✅ Means user was active in both June and July

5️⃣ Final result shows count of such users
✅ Returned as monthly_active_users for month 7
