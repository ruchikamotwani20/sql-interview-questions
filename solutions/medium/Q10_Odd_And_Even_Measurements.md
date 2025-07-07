# Q10 – Odd and Even Measurements
**Source:** Google  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a `measurements` table with:

- `measurement_id`
- `measurement_value`
- `measurement_time`

Each day has multiple measurements.  
Your task is to calculate the **sum of odd-numbered** and **even-numbered** measurements **for each day**:

- 1st, 3rd, 5th, ... → odd
- 2nd, 4th, 6th, ... → even

---

## 🔍 Solution – Using `ROW_NUMBER()` and CASE

```sql
SELECT 
  measurement_day,
  ROUND(
    SUM(CASE WHEN row_num % 2 = 1 THEN measurement_value ELSE 0 END),
    2
  ) AS odd_sum,
  ROUND(
    SUM(CASE WHEN row_num % 2 = 0 THEN measurement_value ELSE 0 END),
    2
  ) AS even_sum
FROM (
  SELECT 
    DATE(measurement_time) AS measurement_day,
    measurement_value,
    ROW_NUMBER() OVER (
      PARTITION BY DATE(measurement_time)
      ORDER BY measurement_time
    ) AS row_num
  FROM measurements
) ordered_measurements
GROUP BY measurement_day
ORDER BY measurement_day;
```

💡 Explanation

The inner query:

Extracts the date (measurement_day)

Assigns a row number to each measurement (per day), ordered by time

The outer query:

Uses CASE to split values into odd and even positions using row_num % 2

Sums both groups and rounds to 2 decimals

Groups by each day

