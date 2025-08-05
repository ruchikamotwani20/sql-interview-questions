# 🧩 SQL Self-Join Problem: Employees with Same Manager

## 📌 Question

**How can you use a self-join to list all pairs of employees who report to the same manager, excluding self-pairs and duplicate pairs?**

---

## 🗃️ Table: `Employees`

| Column Name | Description |
|-------------|-------------|
| emp_id      | Unique employee ID |
| emp_name    | Name of the employee |
| manager_id  | ID of the manager the employee reports to (NULL if top-level) |

### 🔢 Sample Data

| emp_id | emp_name | manager_id |
|--------|----------|------------|
| 1      | Alice    | NULL       |
| 2      | Bob      | 1          |
| 3      | Carol    | 1          |
| 4      | David    | 2          |
| 5      | Eve      | 1          |

---

## 🎯 Goal

> List all **unique pairs of employees** who report to the **same manager**, excluding self-pairs and reversed duplicates.

---

## 🧠 Logic & Explanation
- Self-join the table on `manager_id` to match employees reporting to the same manager:
  
  e1.manager_id = e2.manager_id
   
- Exclude pairs like (Bob, Bob) using:
  
  e1.emp_id < e2.emp_id
  
  This avoids:
  
  Self-pairs
  
  Duplicate reverse pairs (e.g., both Bob–Carol and Carol–Bob)


## Final SQL Query
```sql
SELECT 
    e1.emp_name AS employee1,
    e2.emp_name AS employee2,
    e1.manager_id
FROM 
    Employees e1
JOIN 
    Employees e2
    ON e1.manager_id = e2.manager_id
WHERE 
    e1.emp_id < e2.emp_id;
```

