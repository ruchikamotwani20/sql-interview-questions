# Q38 – Department vs. Company Salary  
**Source:** FAANG  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given two tables:

- `employee`: contains employee details including department.
- `salary`: contains salary payment records per employee.

Your task is to:  
> Compare the **average salary in each department** to the **overall company average salary** for the month of **March 2024**.  
Return:
- `department_id`
- `payment_date` (in `MM-YYYY` format)
- a comparison result as `'Higher'`, `'Lower'`, or `'Same'`.

---

## 🧾 Tables  

### `employee`  
| employee_id | name             | salary | department_id | manager_id |
|-------------|------------------|--------|----------------|------------|

### `salary`  
| salary_id | employee_id | amount | payment_date         |
|-----------|-------------|--------|-----------------------|
| 1         | 1           | 3800   | 2024-03-01 00:00:00   |
| 2         | 2           | 2230   | 2024-03-01 00:00:00   |
| 3         | 3           | 7000   | 2024-03-01 00:00:00   |

---

## 🔍 Solution – Window Functions to Compare Averages

```sql
SELECT DISTINCT 
  e.department_id,
  TO_CHAR(s.payment_date, 'MM-YYYY') AS payment_date,
  CASE 
    WHEN AVG(s.amount) OVER (PARTITION BY e.department_id) < AVG(s.amount) OVER() THEN 'Lower' 
    WHEN AVG(s.amount) OVER (PARTITION BY e.department_id) > AVG(s.amount) OVER() THEN 'Higher' 
    ELSE 'Same'
  END AS comparison
FROM employee e 
JOIN salary s 
  ON e.employee_id = s.employee_id
WHERE s.payment_date BETWEEN '2024-03-01' AND '2024-03-31'
ORDER BY e.department_id;
```
💡 Explanation

- We're joining the employee and salary tables to associate employees with their department and salary amounts.

- AVG(s.amount) OVER (PARTITION BY e.department_id) calculates the average salary per department.

- AVG(s.amount) OVER() calculates the overall average salary across the entire company.

- We compare the two using a CASE statement to classify each department's average as:

  'Higher' than company average

  'Lower' than company average

   or 'Same'

