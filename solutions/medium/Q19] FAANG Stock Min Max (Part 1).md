# Q19 – FAANG Stock Min Max (Part 1)  
**Source:** Bloomberg  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given a table:

- `stock_prices`: contains daily stock data including opening prices for each company.

Your task is to:  
> For each FAANG stock (plus MSFT), return the **month-year of the highest and lowest opening price**, along with the corresponding prices.  
> Format the month as `'Mon-YYYY'` and sort results by `ticker`.

---

## 🧾 Table

### `stock_prices`

| date       | ticker | open   | high   | low    | close  |
|------------|--------|--------|--------|--------|--------|
| 2023-01-31 | AAPL   | 142.28 | 144.34 | 142.70 | 144.29 |
| 2023-02-28 | AAPL   | 146.83 | 149.08 | 147.05 | 147.41 |
| 2023-03-31 | AAPL   | 161.91 | 165.00 | 162.44 | 164.90 |

---

## 🔍 Solution – Using Self JOIN and MAX/MIN Subqueries

```sql
SELECT
  a.ticker,
  TO_CHAR(a.date, 'Mon-YYYY') AS highest_mth,
  a.open AS highest_open,
  TO_CHAR(b.date, 'Mon-YYYY') AS lowest_mth,
  b.open AS lowest_open
FROM stock_prices a
JOIN stock_prices b ON a.ticker = b.ticker
WHERE a.open = (
    SELECT MAX(open) FROM stock_prices WHERE ticker = a.ticker
)
AND b.open = (
    SELECT MIN(open) FROM stock_prices WHERE ticker = b.ticker
)
AND a.ticker IN ('AAPL', 'META', 'AMZN', 'NFLX', 'MSFT', 'GOOG')
ORDER BY a.ticker;
```
💡 Explanation

1️⃣ MAX(open) gives the highest opening price for each stock

2️⃣ MIN(open) gives the lowest opening price for each stock

3️⃣ Self-join on ticker to bring both max and min data side by side

4️⃣ TO_CHAR(..., 'Mon-YYYY') converts the date to month-year format

5️⃣ Final output includes the highest and lowest opening month and price per stock

