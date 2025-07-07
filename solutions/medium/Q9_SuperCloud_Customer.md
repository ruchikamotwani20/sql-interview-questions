# Q9 – Microsoft Supercloud Customers  
**Source:** Microsoft  
**Difficulty:** Medium  

---

## ✅ Goal  
You're given two tables:

- `products(product_id, product_category, product_name)`
- `customer_contracts(customer_id, product_id, amount)`

A **Supercloud Customer** is defined as one who has purchased **at least one product from every product category** listed in the `products` table.

Your task:  
> Return the `customer_id` of all Supercloud customers.

---

## 🔍 Solution

```sql
SELECT cc.customer_id
FROM customer_contracts cc
JOIN products p ON cc.product_id = p.product_id
GROUP BY cc.customer_id
HAVING COUNT(DISTINCT p.product_category) = (
  SELECT COUNT(DISTINCT product_category)
  FROM products
);
```

💡 Explanation

We join customer_contracts with products to get each customer’s product categories.

Then group by customer_id to count how many unique categories each customer has purchased from.

Compare that count to the total number of categories (using a subquery on products).

Only customers with purchases from all categories are returned.

