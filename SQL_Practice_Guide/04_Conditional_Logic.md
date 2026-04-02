# 4. Conditional Logic (CASE)

The `CASE` statement is SQL's version of `If-Else If-Else` logic. It allows you to output different, custom values based on dynamic conditions within your rows.

## 1. Basic CASE usage
```sql
SELECT amount,
    CASE
        WHEN amount >= 300 THEN 'Premium Order'
        WHEN amount >= 100 THEN 'Standard Order'
        ELSE 'Small Order'
    END AS order_tier
FROM orders;
```

*💡 Elaboration:* 
- `CASE` statements evaluate **sequentially** from top to bottom. If the first `WHEN` is true, it assigns the result and skips checking the remaining conditions.
- The `ELSE` clause is optional. However, if you omit an `ELSE`, and none of the `WHEN` conditions match, SQL will output `NULL`.
- ALWAYS remember to close the statement with the `END` keyword. 
- Adding an alias like `AS order_tier` is highly recommended so that the column name in output is clean.

---

## 2. Advanced Usage: Aggregation combined with CASE
You can wrap a `CASE` statement inside aggregate functions for powerful custom analytics—like pivoting data!
```sql
-- Count how many orders fall into each tier category!
SELECT 
    CASE
        WHEN amount >= 300 THEN 'Premium'
        WHEN amount >= 100 THEN 'Standard'
        ELSE 'Small'
    END AS tier,
    COUNT(order_id) as total_orders_in_tier
FROM orders
GROUP BY 
    CASE
        WHEN amount >= 300 THEN 'Premium'
        WHEN amount >= 100 THEN 'Standard'
        ELSE 'Small'
    END;
```

---

## 🧠 Practice Questions

1. Create a query that looks at employee `salary`. If salary > 70000, mark them as 'High Earner'. If they are between 50000 and 70000, mark them as 'Mid Earner'. Else, mark them as 'Entry Level'. Apply the alias 'salary_bracket'.
