# Q36 – Well Paid Employees  
**Source:** FAANG  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given an `employee` table with:

- `employee_id`
- `name`
- `salary`
- `department_id`
- `manager_id`

Your task is to:  
> Find employees who **earn more than their direct managers**.  
> Return the employee's `employee_id` and `name`.

---

## 🧾 Table – `employee`

| employee_id | name             | salary | department_id | manager_id |
|-------------|------------------|--------|----------------|------------|
| 1           | Emma Thompson    | 3800   | 1              | 6          |
| 2           | Daniel Rodriguez | 2230   | 1              | 7          |
| 3           | Olivia Smith     | 7000   | 1              | 8          |
| 4           | Noah Johnson     | 6800   | 2              | 9          |
| 5           | Sophia Martinez  | 1750   | 1              | 11         |
| 6           | Liam Brown       | 13000  | 3              | NULL       |
| 7           | Ava Garcia       | 12500  | 3              | NULL       |
| 8           | William Davis    | 6800   | 2              | NULL       |

---

## 🔍 Solution – Self Join to Compare Salary with Manager

```sql
SELECT e.employee_id, e.name 
FROM employee e
JOIN employee m 
  ON e.manager_id = m.employee_id  -- Match each employee with their manager
WHERE e.salary > m.salary;         -- Check if employee earns more
```
💡 Explanation

We self-join the employee table:

e represents employees.

m represents their managers.

We join using e.manager_id = m.employee_id to pair each employee with their manager.

We filter where e.salary > m.salary to find employees who earn more than their manager.

