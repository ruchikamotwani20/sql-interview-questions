# Q23 – Median Google Search Frequency  
**Source:** Google  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given a table:

- `search_frequency`: shows how many users made a certain number of searches last year.

Your task is to:  
> Calculate the **median number of searches per user**, using the summary table (not raw data).  
> Round the median to **1 decimal place**.

---

## 🧾 Table

### `search_frequency`

| searches | num_users |
|----------|-----------|
| 1        | 2         |
| 2        | 2         |
| 3        | 3         |
| 4        | 1         |

This means:
- 2 users searched 1 time
- 2 users searched 2 times
- 3 users searched 3 times
- 1 user searched 4 times

If we expand this manually:
[1, 1, 2, 2, 3, 3, 3, 4]

→ Median = (2 + 3) / 2 = **2.5**

---

## 🔍 Solution – Using GENERATE_SERIES and PERCENTILE_CONT (PostgreSQL)

```sql
WITH expanded AS (
  SELECT searches
  FROM search_frequency, GENERATE_SERIES(1, num_users)
)

SELECT 
  ROUND(
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY searches), 
    1
  ) AS median;
```
💡 Explanation

1️⃣ GENERATE_SERIES(1, num_users)
→ Expands each row into multiple rows (1 per user)

2️⃣ searches column becomes a full list of all user search counts

3️⃣ PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY searches)
→ Calculates the median of the searches column

4️⃣ ROUND(..., 1)
→ Rounds the median to 1 decimal place
