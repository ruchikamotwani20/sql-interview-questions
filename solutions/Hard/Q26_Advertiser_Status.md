# Q26 – Advertiser Status  
**Source:** Facebook  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given two tables:

- `advertiser`:
  | user_id | status   |
  |-|-|
  | string  | string   |  
  Contains each advertiser's **current** payment status from the previous day.

- `daily_pay`:
  | user_id | paid   |
  |-|-|
  | string  | decimal |  
  Contains advertisers **who made a payment today** (recent payment only).

Your task is to:  
> Determine each advertiser's **new status** based on today's payment behavior, using this logic:

| Current Status | Paid Today? | New Status    |
|----------------|-------------|----------------|
| NEW            | ✅          | EXISTING      |
| NEW            | ❌          | CHURN         |
| EXISTING       | ✅          | EXISTING      |
| EXISTING       | ❌          | CHURN         |
| CHURN          | ✅          | RESURRECT     |
| CHURN          | ❌          | CHURN         |
| RESURRECT      | ✅          | EXISTING      |
| RESURRECT      | ❌          | CHURN         |
| _NULL (new user)_ | ✅      | NEW           |
| _NULL_         | ❌          | CHURN         |

---

## 🧾 Tables

### `advertiser`

| user_id | status     |
|---------|------------|
| bing    | NEW        |
| yahoo   | NEW        |
| alibaba | EXISTING   |
| baidu   | EXISTING   |
| target  | NEW        |

### `daily_pay`

| user_id | paid  |
|---------|-------|
| yahoo   | 45.00 |
| alibaba | 100.00|
| target  | 13.00 |

---

## 🔍 Solution – Using FULL OUTER JOIN, COALESCE & CASE

```sql
SELECT
  COALESCE(a.user_id, d.user_id) AS user_id,
  CASE
    WHEN a.user_id IS NULL AND d.paid IS NOT NULL THEN 'NEW'
    WHEN a.status = 'NEW' AND d.paid IS NOT NULL THEN 'EXISTING'
    WHEN a.status = 'NEW' AND d.paid IS NULL THEN 'CHURN'
    WHEN a.status = 'EXISTING' AND d.paid IS NOT NULL THEN 'EXISTING'
    WHEN a.status = 'EXISTING' AND d.paid IS NULL THEN 'CHURN'
    WHEN a.status = 'CHURN' AND d.paid IS NOT NULL THEN 'RESURRECT'
    WHEN a.status = 'CHURN' AND d.paid IS NULL THEN 'CHURN'
    WHEN a.status = 'RESURRECT' AND d.paid IS NOT NULL THEN 'EXISTING'
    WHEN a.status = 'RESURRECT' AND d.paid IS NULL THEN 'CHURN'
  END AS new_status
FROM advertiser a
FULL OUTER JOIN daily_pay d
  ON a.user_id = d.user_id
ORDER BY user_id;
```
💡 Explanation:

1️⃣ FULL OUTER JOIN merges all advertisers, with or without today's payment.

2️⃣ COALESCE(a.user_id, d.user_id) ensures every user_id appears exactly once.

3️⃣ CASE logic handles every combination of current status and whether they paid today to assign the correct new status.
