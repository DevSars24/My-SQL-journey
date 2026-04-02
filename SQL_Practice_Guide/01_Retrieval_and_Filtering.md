# 1. Retrieval and Filtering

This module covers the core foundation of SQL: fetching data from a database and filtering it down to exactly what you need.

## Scenario Setup
Imagine we have a table called `employees`:
| id | name | age | department | salary |
|---|---|---|---|---|
| 1 | Alice | 28 | IT | 60000 |
| 2 | Bob | 35 | HR | 55000 |
| 3 | Charlie | 22 | IT | 45000 |
| 4 | Diana | 40 | Finance | 80000 |
| 5 | Eva | 29 | HR | 52000 |

---

## 1. `SELECT` - Fetching Data
The `SELECT` statement retrieves data from the database.
```sql
-- Select specific columns
SELECT name, department FROM employees;

-- Select EVERYTHING from the table
SELECT * FROM employees;
```
*💡 Elaboration:* Using `SELECT *` is great for quick checks, but in real production codebases, you should explicitly name the columns you want. This reduces memory usage and speeds up server response times.

---

## 2. `WHERE` - Filtering Data
The `WHERE` clause filters rows based on a specific condition.
```sql
SELECT * FROM employees WHERE age > 25;
```

---

## 3. Logical Operators: `AND`, `OR`, `NOT`
These allow you to combine or invert multiple conditions.
```sql
-- AND: Both conditions must be strictly true
SELECT * FROM employees WHERE age > 25 AND department = 'IT';

-- OR: At least one condition must be true
SELECT * FROM employees WHERE age > 25 OR department = 'IT';

-- NOT: Reverses the condition to exclude data
SELECT * FROM employees WHERE NOT department = 'Finance';
```

---

## 4. `BETWEEN` - Range Filtering
Selects values within a specified range. It is inclusive (meaning the boundaries are included in the results).
```sql
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 70000;
```
*💡 Elaboration:* This is much cleaner and easier to read than writing `WHERE salary >= 50000 AND salary <= 70000`.

---

## 5. `IN` - Multiple Exact Matches
Matches a row's value against a list of specific values.
```sql
SELECT * FROM employees WHERE department IN ('IT', 'Finance');
```
*💡 Elaboration:* Prefer `IN` over multiple chained `OR` statements for better readability. (e.g., `department = 'IT' OR department = 'Finance'` is verbose).

---

## 6. `LIKE` - Pattern Matching
Used for partial text searches when you don't know the exact string.
- `%` matches zero or more characters.
- `_` matches exactly one character.

```sql
-- Name starts with 'A' followed by anything
SELECT * FROM employees WHERE name LIKE 'A%';

-- Name contains 'ar' anywhere in the string
SELECT * FROM employees WHERE name LIKE '%ar%';

-- Name has exactly 3 letters and ends with 'ob'
SELECT * FROM employees WHERE name LIKE '_ob';
```

---

## 🧠 Practice Questions
*Can you write the queries for these scenarios? Try it out!*

1. Write a query to find all employees in the 'IT' department earning more than 50,000.
2. Write a query to find all employees whose name ends with the letter 'a'.
3. Write a query to find employees who are NOT in the 'IT' department and are aged between 25 and 35.
