# Q3 – Second Highest Salary  
**Source:** FAANG  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given an `employee` table with at least these columns:
- `employee_id`
- `name`
- `salary`

You are asked to return the **second highest distinct salary** in the company.

**Note:** If multiple employees have the same second highest salary, return the salary only once.

---

## 🔍 Method 1 – Using `OFFSET` and `LIMIT`

```sql
SELECT DISTINCT salary AS second_highest_salary
FROM employee
ORDER BY salary DESC
OFFSET 1
LIMIT 1;
```

💡 Explanation

ORDER BY salary DESC: sorts salaries in descending order

OFFSET 1: skips the highest salary

LIMIT 1: picks the second distinct value

DISTINCT: removes duplicate salaries

## 🔍 Method 2 – Using MAX and a Subquery

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employee
WHERE salary < (
    SELECT MAX(salary)
    FROM employee
);
```

💡 Explanation

Inner query: finds the highest salary in the company

Outer query: finds the maximum salary that is less than the highest → i.e., the second highest

Works even if the top salary is shared by multiple people

