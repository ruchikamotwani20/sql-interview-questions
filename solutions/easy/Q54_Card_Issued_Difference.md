# Q54 – Card Issue Difference  
**Source:** JP Morgan
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a table `monthly_cards_issued` with:

- `card_name`: name of the credit/debit card
- `issued_amount`: number of cards issued each month

Your task is to:  
> For each card type, calculate the **difference between the highest and lowest monthly issued amounts**.  
Return the card name and the difference, ordered by the difference in descending order.

---

## 🧾 Table  

### `monthly_cards_issued`  
| card_name     | issued_amount |
|---------------|----------------|
| Gold Card     | 1000           |
| Gold Card     | 500            |
| Silver Card   | 1200           |
| Silver Card   | 600            |

---

## 🔍 Solution – Aggregate with MAX - MIN

```sql
SELECT 
    card_name,
    MAX(issued_amount) - MIN(issued_amount) AS difference
FROM 
    monthly_cards_issued
GROUP BY 
    card_name
ORDER BY 
    difference DESC;
```

💡 Explanation

Group the table by card_name.

Use MAX() and MIN() to find the highest and lowest number of cards issued per card.

Subtract to find the difference.

Sort the result in descending order of difference.
