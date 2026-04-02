# 6. Comprehensive Master Practice

Now that you've learned the components, it's time to put it all together to solve business logic! 
Head over to [DB Fiddle](https://www.db-fiddle.com/), copy the schema script below into the **Schema** panel, and solve the challenges in the **Query** panel!

## Setup Script
```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT,
    dept_id INT,
    hire_date DATE
);

INSERT INTO departments VALUES (1, 'HR'), (2, 'Engineering'), (3, 'Sales');

INSERT INTO employees VALUES 
(101, 'Alice', 60000, 1, '2022-01-15'),
(102, 'Bob', 80000, 2, '2021-03-10'),
(103, 'Charlie', 55000, 3, '2023-05-20'),
(104, 'Diana', 90000, 2, '2020-11-05'),
(105, 'Eve', 45000, 3, '2023-12-01'),
(106, 'Frank', 75000, NULL, '2024-01-01');
```

---

## 🎯 The Challenges

### **Beginner Tier**
1. Retrieve the names of all employees and sort them alphabetically from A-Z.
2. Find all employees whose salary is physically between 50,000 and 70,000 (inclusive).
3. Find the total number (count) of employees in the entire company.

### **Intermediate Tier**
4. What is the average salary of the 'Engineering' department specifically? *(Requires an INNER JOIN and AVG()).*
5. List the department names alongside the total combined salary budget for each department. *(Requires JOIN and GROUP BY).*
6. Find all employees who were hired in the year 2023. *(Hint: Use `BETWEEN '2023-01-01' AND '2023-12-31'` or use `LIKE '2023%'`).*

### **Advanced Tier**
7. Write a query that groups employees into pay tiers:
   - Salary >= 80000 -> 'Senior Pay'
   - Salary BETWEEN 55000 AND 79999 -> 'Mid Pay'
   - Salary < 55000 -> 'Junior Pay'
   Your result should only have two columns: The custom pay tier name, and the count of employees within that tier.
8. Find employees who do NOT belong to any department. *(Hint: Look closely at employee 106 and do a proper NULL check).*
9. Show all department names, including those that currently have ZERO employees. *(Hint: You will need a `LEFT JOIN` or `RIGHT JOIN` based on the order of your tables).*

---
*Take your time, break the complex queries down into smaller pieces step-by-step. Mastering these fundamentals will conquer 90% of your daily SQL needs!*
