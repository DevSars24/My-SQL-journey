# 3. Math Operations and NULL Handling

Data is rarely perfectly clean. This module covers mathematically transforming data on the fly and dealing with missing data.

## 1. Arithmetic & Math Functions
You can perform math row-by-row on your data directly inside the `SELECT` statement.
```sql
-- Adding a flat $10 shipping fee to every order
SELECT amount, amount + 10 AS amount_with_shipping FROM orders;

-- Calculating a 10% tax rate
SELECT amount, amount * 0.10 AS tax FROM orders;

-- Rounding numbers (to exactly 2 decimal places)
SELECT amount, ROUND(amount * 1.15, 2) AS total_with_tax FROM orders;
```
*💡 Other Useful Math Functions:* 
- `CEIL(number)`: Always rounds UP to the nearest whole integer.
- `FLOOR(number)`: Always rounds DOWN to the nearest whole integer.
- `ABS(number)`: Returns the absolute (positive) value.

---

## 2. Division & Nested Averages
SQL handles division using the `/` operator.
```sql
-- Calculating Average sales manually
SELECT SUM(amount) / COUNT(order_id) AS manual_average FROM orders;
```
*Warning:* Be careful when dividing two integers in some databases (like SQL Server)—it might perform integer division (dropping the decimals).

---

## 3. Handling NULL Values
`NULL` means "unknown", "blank", or "missing". It is NOT the same as zero or an empty string. You cannot use `=` or `!=` to check for NULL.
```sql
-- WRONG way (This will always return nothing)
SELECT * FROM orders WHERE amount = NULL;

-- CORRECT way to find missing amounts
SELECT * FROM orders WHERE amount IS NULL;

-- CORRECT way to find populated, valid amounts
SELECT * FROM orders WHERE amount IS NOT NULL;
```

---

## 4. `COALESCE` - Safely Replacing NULLs
`COALESCE(val1, val2, ...)` goes through a list and returns the first non-NULL value. This is highly utilized in production systems to provide default fallback values to prevent errors.
```sql
-- If an order's amount is missing (NULL), it will be treated as $0
SELECT order_id, COALESCE(amount, 0) AS safe_amount FROM orders;
```

---

## 🧠 Practice Questions

1. Select the `amount` column, and formulate another column showing the `amount` discounted by 20%, rounded to exactly 1 decimal place.
2. Using the `employees` table from Module 1, write a query to find all employees where the department is strictly unknown/missing.
