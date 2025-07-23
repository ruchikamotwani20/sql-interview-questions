# Find the 3rd Highest Salary (No LIMIT / TOP)

**Table:** `Employees`  
**Columns:** `emp_id`, `emp_name`, `salary`

---

## ❓ Question

Write an SQL query to find the **3rd highest distinct salary** from the `Employees` table **without using** `LIMIT`, `TOP`, or `ROWNUM`.

---

## Solution 1: Nested MAX Queries

```sql
SELECT MAX(salary)
FROM Employees
WHERE salary NOT IN (
    SELECT MAX(salary)
    FROM Employees
    WHERE salary NOT IN (
        SELECT MAX(salary)
        FROM Employees
    )
);
```

🔍 Explanation

- Innermost MAX(salary): Highest salary

- Second MAX(salary) with NOT IN: Second highest

- Outermost MAX(salary) with NOT IN: Third highest

## Solution 2: Using DENSE_RANK() (if window functions are allowed)

```sql
SELECT salary
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employees
) AS ranked
WHERE rnk = 3;
```

📌 What is DENSE_RANK()?

Assigns rank without gaps for ties.

Example: If two people have the same salary, they share the same rank.

No skipped ranks (unlike RANK()).

