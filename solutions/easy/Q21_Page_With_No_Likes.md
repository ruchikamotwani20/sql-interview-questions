# Q21 – Page With No Likes  
**Source:** Facebook  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given two tables:

- `pages`: list of all Facebook pages  
- `page_likes`: records of likes on pages

Your task is to:  
> Find all `page_id`s from the `pages` table that **have no likes** in the `page_likes` table.

---

## 🧾 Tables

### `pages`

| page_id |
|---------|
| 1       |
| 2       |
| 3       |
| 4       |

### `page_likes`

| user_id | page_id |
|---------|---------|
| 101     | 1       |
| 102     | 2       |
| 103     | 2       |

---

## 🔍 Solution – Method 1: FULL JOIN and NULL Filter

```sql
SELECT pa.page_id
FROM pages pa
FULL JOIN page_likes pl 
  ON pa.page_id = pl.page_id 
WHERE pl.page_id IS NULL
ORDER BY pa.page_id;
```

## 🔁 Alternative – Method 2: Using EXCEPT
```sql
SELECT page_id
FROM pages
EXCEPT
SELECT page_id
FROM page_likes
ORDER BY page_id;
```
