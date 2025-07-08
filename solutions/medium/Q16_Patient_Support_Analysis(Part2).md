# Q16 – Patient Support Analysis (Part 2): Uncategorised Calls  
**Source:** UnitedHealth Group (Advocate4Me Program)  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table `callers` with:

- `policy_holder_id`: ID of the member  
- `case_id`: unique call identifier  
- `call_category`: type of support requested  
- `call_date`: when the call was made  
- `call_duration_secs`: length of the call in seconds  

Your task is to:  
> Calculate the **percentage of calls that are uncategorised**, where the `call_category` is either `'n/a'` or **empty (NULL)**.  
> Round the result to **1 decimal place**.

---

## 🧾 Sample Table – `callers`

| policy_holder_id | case_id | call_category         | call_date             | call_duration_secs |
|------------------|---------|------------------------|------------------------|---------------------|
| 1                | ...     | emergency assistance   | 2023-04-13T19:16:53Z   | 144                 |
| 1                | ...     | authorisation          | 2023-05-25T09:09:30Z   | 815                 |
| 2                | ...     | n/a                    | 2023-01-26T01:21:27Z   | 992                 |
| 2                | ...     | emergency assistance   | 2023-03-09T10:58:54Z   | 128                 |
| 2                | ...     | benefits               | 2023-06-05T07:35:43Z   | 619                 |

---

## 🔍 Solution – Using `CASE`, `SUM`, `COUNT`, and `ROUND`

```sql
SELECT 
  ROUND(
    100.0 * SUM(CASE 
                 WHEN call_category = 'n/a' OR call_category IS NULL THEN 1 
                 ELSE 0 
               END) 
    / COUNT(*), 1
  ) AS uncategorised_call_pct
FROM callers;
```

💡 Explanation

1️⃣ CASE ... THEN 1 ELSE 0
Marks a call as uncategorised if:

call_category = 'n/a'

call_category IS NULL

2️⃣ SUM(...)
Counts the total number of uncategorised calls

3️⃣ COUNT(*)
Counts all calls

4️⃣ 100.0 * ... / ...
Calculates the percentage

5️⃣ ROUND(..., 1)
Rounds the result to 1 decimal
