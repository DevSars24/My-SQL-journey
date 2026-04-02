# 2. Sorting and Aggregation

Once we filter our data, we often need to sort it or perform statistical calculations on groups of data.

## Scenario Setup
Using an `orders` table tracking e-commerce sales:
| order_id | customer_id | amount | order_date |
|---|---|---|---|
| 1 | 101 | 150.00 | 2024-01-01 |
| 2 | 102 | 200.00 | 2024-01-02 |
| 3 | 101 | 300.00 | 2024-01-03 |
| 4 | 103 | 50.00 | 2024-01-04 |

---

## 1. `ORDER BY`
Sorts the output of your query. `ASC` is default (smallest to largest / A-Z), `DESC` reverses it (largest to smallest / Z-A).
```sql
SELECT * FROM orders ORDER BY amount DESC;
```

---

## 2. Aggregations: `SUM`, `AVG`, `COUNT`
These functions combine multiple rows into a single calculated value.
```sql
-- Total amount of all sales made
SELECT SUM(amount) AS total_sales FROM orders;

-- Average sale amount across all orders
SELECT AVG(amount) AS average_sale FROM orders;

-- Number of total orders placed
SELECT COUNT(order_id) AS total_orders FROM orders;
```

---

## 3. `GROUP BY`
Used with aggregate functions to group rows that share a property. Instead of one grand total, you get a total *per category*.
```sql
-- Total amount spent BY EACH individual customer
SELECT customer_id, SUM(amount) AS total_spent 
FROM orders 
GROUP BY customer_id;
```
*💡 Elaboration:* A golden rule in SQL: **Any column in your `SELECT` clause that is NOT inside an aggregate function (like `SUM()`) MUST be included in your `GROUP BY` clause.**

---

## 4. `HAVING`
Filters **grouped** data. You cannot use `WHERE` to filter aggregate results.
```sql
-- Customers who have spent more than $200 in TOTAL lifetime sales
SELECT customer_id, SUM(amount) AS total_spent 
FROM orders 
GROUP BY customer_id
HAVING SUM(amount) > 200;
```
*💡 Elaboration:* Think of `WHERE` as filtering individual rows *before* grouping, and `HAVING` as filtering the aggregated categories *after* grouping.

---

## 5. `DISTINCT`
Removes duplicate rows from the results, giving you unique values.
```sql
-- Get a list of unique customer IDs that have placed orders
SELECT DISTINCT customer_id FROM orders;
```

---

## 🧠 Practice Questions

1. Calculate the total number of unique customers who placed an order in the database.
2. Find the highest (`MAX`) order amount for each customer, sorted by the amount in descending order.
3. Query the `orders` table to find which customers have placed more than 1 order (Hint: use `HAVING COUNT()`).
