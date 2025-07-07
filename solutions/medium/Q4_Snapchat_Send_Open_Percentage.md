# Q4 – Snapchat: Send vs. Open Snap Percentage by Age Group  
**Source:** Snapchat  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given two tables:

- `activities`: contains user activity types like `'send'`, `'open'`, `'chat'`, and time spent on each  
- `age_breakdown`: maps `user_id` to `age_bucket` like `'21-25'`, `'26-30'`

Your task is to:
- For each age group (`age_bucket`), calculate:
  - Percentage of time spent **sending** snaps
  - Percentage of time spent **opening** snaps

Only include `send` and `open` activities in the calculations.

---

## 🔍 Solution

```sql
SELECT 
  ab.age_bucket,
  ROUND(
    100 * SUM(CASE WHEN a.activity_type = 'send' THEN a.time_spent ELSE 0 END) / SUM(a.time_spent),
    2
  ) AS send_perc,
  ROUND(
    100 * SUM(CASE WHEN a.activity_type = 'open' THEN a.time_spent ELSE 0 END) / SUM(a.time_spent),
    2
  ) AS open_perc
FROM activities a
JOIN age_breakdown ab 
  ON a.user_id = ab.user_id
WHERE a.activity_type IN ('send', 'open')
GROUP BY ab.age_bucket
ORDER BY ab.age_bucket;
```

💡 Explanation

We join activities with age_breakdown to get the age group of each user.

Use CASE WHEN inside SUM() to isolate time spent on each activity.

Divide by total time spent per age group, and round to 2 decimal places.

Filter to only include 'send' and 'open' activities (exclude 'chat' etc.)
