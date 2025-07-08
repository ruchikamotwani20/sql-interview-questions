# Q13 – Cards Issued in Launch Month  
**Source:** JPMorgan Chase  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table `monthly_cards_issued` with:

- `card_name`: name of the credit card  
- `issue_year` & `issue_month`: when cards were issued  
- `issued_amount`: how many were issued that month  

Your task is to:  
> Return each card’s name along with the number of cards issued in **its launch month** — the **first month it appeared** in the data.  
> Output should be sorted by `issued_amount` in descending order.

---

## 🔍 Solution – Using `ROW_NUMBER()` and Subquery

```sql
SELECT card_name, issued_amount
FROM (
    SELECT 
        card_name,
        issue_year,
        issue_month,
        issued_amount,
        ROW_NUMBER() OVER (
            PARTITION BY card_name
            ORDER BY issue_year, issue_month
        ) AS rnk
    FROM monthly_cards_issued
) AS ranked
WHERE rnk = 1
ORDER BY issued_amount DESC;
```

💡 Explanation

1️⃣ ROW_NUMBER() OVER (PARTITION BY card_name ORDER BY issue_year, issue_month)
This assigns a row number starting from 1 for each card, based on its earliest appearance (year, then month).
→ rnk = 1 will be the launch month for that card.

2️⃣ The outer query filters only those rows where rnk = 1
This means you're keeping only the first month when each card was issued.

3️⃣ Final ORDER BY issued_amount DESC
Sorts the results so the cards with the highest launch issuance come first.
