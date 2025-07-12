# Q24 – Unfinished Parts  
**Source:** Tesla  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table:

- `parts_assembly`: contains information about parts and their assembly steps, along with their finish dates.

Your task is to:  
> Find all parts that have **not yet been finished** (i.e., `finish_date` is `NULL`).  
> Return the `part` and `assembly_step`.

---

## 🧾 Table

### `parts_assembly`

| part      | assembly_step | finish_date |
|-----------|----------------|-------------|
| battery   | 1              | 2023-05-02  |
| motor     | 2              |      |
| door      | 3              | 2023-05-03  |

---

## 🔍 Solution – Using NULL Filter

```sql
SELECT part, assembly_step 
FROM parts_assembly
WHERE finish_date IS NULL;
