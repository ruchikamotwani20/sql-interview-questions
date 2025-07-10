# Q18 – Data Science Skills  
**Source:** LinkedIn  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table:

- `candidates`: contains `candidate_id` and their listed `skills`.

Your task is to:  
> Find all candidates who have **all three** of the following skills:  
> - `Python`  
> - `Tableau`  
> - `PostgreSQL`  
Return their `candidate_id`s in ascending order.

---

## 🧾 Table

### `candidates`

| candidate_id | skill       |
|--------------|-------------|
| 101          | Python      |
| 101          | Tableau     |
| 101          | PostgreSQL  |
| 102          | Python      |
| 102          | Tableau     |
| 103          | Tableau     |
| 103          | PostgreSQL  |

---

## 🔍 Solution – Method 1: GROUP BY and HAVING

```sql
SELECT candidate_id
FROM candidates
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(DISTINCT skill) = 3
ORDER BY candidate_id;
```
## 🔁 Alternative – Method 2: Using INTERSECT

```sql
SELECT candidate_id FROM candidates WHERE skill = 'Python'
INTERSECT 
SELECT candidate_id FROM candidates WHERE skill = 'Tableau'
INTERSECT  
SELECT candidate_id FROM candidates WHERE skill = 'PostgreSQL'
ORDER BY 1;
