# Find Employees Who Don’t Have Any Direct Reports

You are given a table named `Employees` with the following structure:

| Column Name | Type     |
|-------------|----------|
| emp_id      | int      |
| emp_name    | varchar  |
| salary      | int      |
| manager_id  | int      |

- `emp_id` is the unique identifier for each employee.
- `manager_id` refers to the `emp_id` of that employee’s manager.
- If `manager_id` is `NULL`, that employee has no manager.

### ❓Task:
Write an SQL query to **find the names of all employees who do NOT manage anyone**, i.e., those who have **no direct reports**.

---

## 🧠 Approach

To find employees who don’t manage anyone:
- We look for `emp_id`s that **do not appear** in the `manager_id` column of any other row.
- This is because if someone is a manager, their `emp_id` will show up as `manager_id` for someone else.

---

## ✅ SQL Solution

```sql
SELECT emp_name 
FROM employees 
WHERE emp_id NOT IN (
    SELECT manager_id 
    FROM employees 
    WHERE manager_id IS NOT NULL
);
```
## 📊 Example
Given this employees table:

| emp_id | emp_name | salary  | manager_id |
|--------|----------|---------|------------|
| 1      | Alice    | 100000  | NULL       |
| 2      | Bob      | 80000   | 1          |
| 3      | Carol    | 70000   | 1          |
| 4      | David    | 75000   | 2          |

## 👇 Output:
emp_name

Carol

David

## 🔍 Explanation
Alice (emp_id = 1) is a manager (since Bob and Carol report to her).

Bob (emp_id = 2) is also a manager (David reports to him).

Carol and David are not listed as manager_id for anyone → they don’t manage anyone.

So they are the correct result.

## 🏁 Summary
This problem teaches how to:

Use a NOT IN subquery.

Understand hierarchical relationships using self-referencing columns (manager_id).

