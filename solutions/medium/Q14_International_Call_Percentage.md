# Q14 – Percentage of International Calls  
**Source:** Telecommunications (e.g., AT&T, Verizon)  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given two tables:

- `phone_calls`: contains who called whom and when  
- `phone_info`: contains the country of each caller_id  

Your task is to:  
> Calculate the **percentage of calls that are international**, where the **caller and receiver are in different countries**.  
> Round the result to **1 decimal place**.

---

## 🧾 Tables

### `phone_calls`

| caller_id | receiver_id | call_time             |
|-----------|-------------|------------------------|
| 1         | 2           | 2022-07-04 10:13:49    |
| 1         | 5           | 2022-08-21 23:54:56    |
| 5         | 1           | 2022-05-13 17:24:06    |
| 5         | 6           | 2022-03-18 12:11:49    |

### `phone_info`

| caller_id | country_id |
|-----------|------------|
| 1         | US         |
| 2         | US         |
| 5         | IN         |
| 6         | IN         |

---

## 🔍 Solution – Using JOIN and CASE

```sql
SELECT 
  ROUND(
    100.0 * SUM(CASE 
                 WHEN caller.country_id != receiver.country_id THEN 1 
                 ELSE 0 
               END) 
    / COUNT(*), 1
  ) AS international_calls_pct
FROM phone_calls pc 
JOIN phone_info caller
  ON pc.caller_id = caller.caller_id
JOIN phone_info receiver 
  ON pc.receiver_id = receiver.caller_id;
```

💡 Explanation

1️⃣ JOIN phone_info caller
Find the country of the caller

2️⃣ JOIN phone_info receiver
Find the country of the receiver

3️⃣ CASE WHEN caller.country_id != receiver.country_id THEN 1 ELSE 0 END
Marks the call as international (1) or not (0)

4️⃣ SUM(...) / COUNT(*)
Gives the percentage of international calls

5️⃣ ROUND(..., 1)
Rounds the result to 1 decimal
