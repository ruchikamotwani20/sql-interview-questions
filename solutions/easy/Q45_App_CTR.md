# Q45 – App CTR  
**Source:** Facebook  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table `events` with:

- `app_id`
- `event_type` (either `'click'` or `'impression'`)
- `timestamp`

Your task is to:  
> Calculate the **Click-Through Rate (CTR)** for each `app_id` for the year **2022**.  
CTR is defined as:  
**(clicks ÷ impressions) × 100**, rounded to 2 decimal places.

Return each app's ID and its CTR, ordered by CTR in ascending order.

---

## 🧾 Table  

### `events`  
| app_id | event_type  | timestamp             |
|--------|-------------|------------------------|
| 1      | click       | 2022-01-03 00:00:00    |
| 1      | impression  | 2022-01-03 00:00:01    |
| 2      | impression  | 2022-02-01 00:00:00    |

---

## 🔍 Solution – Aggregation with CASE + NULLIF

```sql
SELECT DISTINCT 
  app_id,
  ROUND(
    SUM(CASE WHEN event_type = 'click' THEN 1 ELSE 0 END) * 100.0
    / NULLIF(SUM(CASE WHEN event_type = 'impression' THEN 1 ELSE 0 END), 0),
    2
  ) AS ctr
FROM events 
WHERE EXTRACT(YEAR FROM timestamp) = 2022
GROUP BY app_id
ORDER BY ctr;
```
💡 Explanation

We filter only events from 2022 using EXTRACT(YEAR FROM timestamp).

We count the number of clicks and impressions per app_id.

We use NULLIF to avoid division by zero.

ROUND(..., 2) ensures the result is to 2 decimal places.


