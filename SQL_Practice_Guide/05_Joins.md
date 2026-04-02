# 5. Joins - Combining Tables

Real-world databases are highly "normalized," meaning data is split across multiple tables to prevent duplication. `JOIN` is how we link data back together using shared relationships (Usually Primary Keys & Foreign Keys).

## Scenario Setup
Table: `customers`
| customer_id | name |
|---|---|
| 101 | Alice |
| 102 | Bob |
| 103 | Charlie |

Table: `orders`
| order_id | customer_id | amount |
|---|---|---|
| 1 | 101 | 150 |
| 2 | 101 | 300 |
| 3 | 104 | 500 |

---

## 1. `INNER JOIN` (The standard JOIN)
Returns ONLY rows that have a match in **BOTH** tables. Data missing from either side is completely excluded.
```sql
SELECT customers.name, orders.amount
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id;
```
*💡 Result Explanation:* Includes Alice (150) and Alice (300). Bob, Charlie, and foreign customer 104 are left out because they lack an opposite matching record.

---

## 2. `LEFT JOIN`
Returns **ALL** rows from the LEFT table (the first table you write), and the matched rows from the right table. If there is no right-side match, it fills the missing columns with `NULL`.
```sql
SELECT customers.name, orders.amount
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id;
```
*💡 Result Explanation:* Alice's orders appear correctly. Bob and Charlie are also printed but show `NULL` for amount, because they haven't placed an order. Customer 104 is still completely excluded.

---

## 3. `RIGHT JOIN`
Returns **ALL** rows from the RIGHT table (the second table). Unmatched left data becomes NULL.
```sql
SELECT customers.name, orders.amount
FROM customers
RIGHT JOIN orders ON customers.customer_id = orders.customer_id;
```
*💡 Result Explanation:* Includes the 500 amount order for customer 104, however, the customer 'name' column will be `NULL` because 104 doesn't exist in the customers table.

---

## 4. `FULL JOIN`
Returns ALL records whenever there is a match in **EITHER** the left or right table. (Effectively a combination of LEFT and RIGHT joins). Does not work natively in MySQL without a UNION.

---

## 🧠 Practice Questions

1. Which customers have NOT placed an order yet? Write a query to find out. *(Hint: Use a `LEFT JOIN` and check `WHERE orders.amount IS NULL`)*.
2. Show the sum amount spent by each unique customer `name` *(Requires combining JOIN with GROUP BY)*.
