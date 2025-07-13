# Q35 – 3-Topping Pizza  
**Source:** McKinsey  
**Difficulty:** Hard  

---

## ✅ Goal  
You're given a `pizza_toppings` table with:

- `topping_name`: name of the topping  
- `ingredient_cost`: cost of that topping  

Your task is to:  
> List **all unique 3-topping combinations**, calculate the **total cost** for each, and sort:  
> 1. **Descending by cost**  
> 2. **Alphabetically by topping combo**  

---

## 🧾 Table – `pizza_toppings`

| topping_name   | ingredient_cost |
|----------------|------------------|
| Pepperoni      | 0.50             |
| Sausage        | 0.70             |
| Chicken        | 0.55             |
| Extra Cheese   | 0.40             |

---

## 🔍 Solution – Self Join with Alphabetical Constraints

```sql
SELECT 
  CONCAT(t1.topping_name, ',', t2.topping_name, ',', t3.topping_name) AS pizza,
  ROUND(t1.ingredient_cost + t2.ingredient_cost + t3.ingredient_cost, 2) AS total_cost
FROM pizza_toppings t1
JOIN pizza_toppings t2 
  ON t1.topping_name < t2.topping_name
JOIN pizza_toppings t3 
  ON t2.topping_name < t3.topping_name
ORDER BY 
  total_cost DESC,
  pizza ASC;
```
💡 Explanation

1️⃣ t1.topping_name < t2.topping_name and t2.topping_name < t3.topping_name
Ensure no duplicates or repeated toppings, and all names appear in alphabetical order.

2️⃣ CONCAT(...)
Concatenates topping names into a single string like 'Chicken,Pepperoni,Sausage'.

3️⃣ ROUND(..., 2)
Calculates and rounds the total cost of each 3-topping pizza.

4️⃣ ORDER BY total_cost DESC, pizza ASC
Sorts by most expensive pizzas first; breaks ties alphabetically.
