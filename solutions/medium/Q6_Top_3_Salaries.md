# Q6 – Top 3 Salaries 
**Source:** FAANG  
**Difficulty:** Medium  

---

## ✅ Goal  
Given two tables:

- `employee(employee_id, name, salary, department_id)`
- `department(department_id, department_name)`

Return the **top 3 highest-paid employees** in each department.

If multiple employees share the same salary, they should be ranked using `DENSE_RANK()`.  
The final output should include:
- `department_name`
- `name`
- `salary`

Sorted by:
1. `department_name` (A-Z)  
2. `salary` (high to low)  
3. `name` (A-Z) if salaries tie

---

## 🔍 Solution – Using `DENSE_RANK()` and Subquery

```sql
SELECT 
  dept.department_name, 
  emp.name, 
  emp.salary
FROM (
  SELECT 
    e.*,
    DENSE_RANK() OVER (
      PARTITION BY e.department_id 
      ORDER BY e.salary DESC
    ) AS rnk
  FROM employee e
) emp
JOIN department dept 
  ON emp.department_id = dept.department_id
WHERE emp.rnk <= 3
ORDER BY 
  dept.department_name, 
  emp.salary DESC, 
  emp.name;
```

💡 Explanation

We use DENSE_RANK() to assign salary-based rankings within each department.

In the outer query, we filter to only keep the top 3 ranked employees per department.

Finally, we join with the department table to get the department name.

Sorting by department → salary → name ensures stable and expected ordering.
