# Q27 – Laptop V/S Mobile Viewership  
**Source:** NY Times  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table `viewership` that records the device type used by users to access the platform.

Your task is to:  
> Calculate the total number of views from **laptops** and from **mobile devices**, where mobile devices include **phones and tablets**.

Return the result as two columns:  
- `laptop_views`  
- `mobile_views`

---

## 🧾 Table

### `viewership`

| user_id | device_type | view_time           |
|---------|-------------|---------------------|
| 123     | tablet      | 2022-01-02 00:00:00 |
| 125     | laptop      | 2022-01-07 00:00:00 |
| 128     | laptop      | 2022-02-09 00:00:00 |
| 129     | phone       | 2022-02-09 00:00:00 |
| 145     | tablet      | 2022-02-24 00:00:00 |

---

## 🔍 Solution

### ✅ METHOD 01 – Using `FILTER`

```sql
SELECT 
  COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
  COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone')) AS mobile_views 
FROM viewership;
```

### ✅ METHOD 02 – Using CASE WHEN
```sql
SELECT 
  SUM(CASE WHEN device_type = 'laptop' THEN 1 ELSE 0 END) AS laptop_views, 
  SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN 1 ELSE 0 END) AS mobile_views 
FROM viewership;
```
