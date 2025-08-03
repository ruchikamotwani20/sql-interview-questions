# SQL Problem: Find Sequences of Consecutive Numbers

**Problem:**  
Write a query to find sequences where the same number appears **at least four times consecutively** in a column.

**Table:** `Logs`  
Columns:  
- `id` (int) – unique identifier of the row  
- `num` (int) – number to analyze  

---

## Approach 1: Using Window Functions (LEAD)

```sql
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT num,
           LEAD(num, 1) OVER (ORDER BY id) AS next1,
           LEAD(num, 2) OVER (ORDER BY id) AS next2,
           LEAD(num, 3) OVER (ORDER BY id) AS next3
    FROM Logs
) t
WHERE num = next1 AND num = next2 AND num = next3;
```

## Explanation:

LEAD(): Fetches values from upcoming rows within the same ordered set.

We check if the current row's num equals the next three rows' num.

If true, it means there are at least 4 consecutive occurrences of this number.

DISTINCT removes duplicates so each qualifying number is returned only once.

## Approach 2: Using Self-Join

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id = l2.id - 1 AND l1.num = l2.num
JOIN Logs l3 ON l1.id = l3.id - 2 AND l1.num = l3.num
JOIN Logs l4 ON l1.id = l4.id - 3 AND l1.num = l4.num;
```

## Explanation:

We join the table with itself 3 times to compare each row with the next three rows.

If all 4 rows (l1, l2, l3, l4) have the same num, it forms a sequence of at least 4 consecutive numbers.

Use DISTINCT to ensure unique numbers in the result.

