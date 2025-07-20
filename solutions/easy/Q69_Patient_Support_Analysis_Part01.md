# Q69 – Patient Support Analysis (Part 01)  
**Source:** UnitedHealth  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a `callers` table with multiple support call records. Each row represents a call made by a policyholder.

Your task is to:  
> Find how many **unique policy holders** have made **at least 3 calls** to the support center.

---

## 🧾 Table – `callers`

| policy_holder_id |
|------------------|
| 101              |
| 101              |
| 102              |
| 101              |
| 103              |
| 104              |
| 104              |
| 104              |

---

## 🔍 Solution – GROUP BY and COUNT

```sql
SELECT COUNT(*) AS policy_holder_count
FROM (
  SELECT policy_holder_id
  FROM callers
  GROUP BY policy_holder_id
  HAVING COUNT(*) >= 3
) AS sub;
```
💡 Explanation

- Group the records by policy_holder_id.

- Use HAVING COUNT(*) >= 3 to filter those with 3 or more calls.

- Wrap it in a subquery and count how many policy holders satisfy this condition.
