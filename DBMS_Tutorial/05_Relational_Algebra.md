# 5. Relational Algebra and Calculus

## 📖 The Core Concept
Before SQL was invented, Edgar F. Codd created a theoretical mathematical language called **Relational Algebra**. If SQL is "English", Relational Algebra is the "Math equation" running in the background.

---

## ➗ The Basic Operators (The Scenarios)

### 1. Selection ($\sigma$) -> The Row Filter (Equivalent to SQL `WHERE`)
- **Scenario:** Imagine sitting in a stadium and pointing a flashlight ONLY at people wearing Red shirts. 
- **What it does:** Extracts entire **rows** that meet a certain condition.
- **Example:** $\sigma_{\text{age} > 20}(\text{Students})$

### 2. Projection ($\pi$) -> The Column Filter (Equivalent to SQL `SELECT`)
- **Scenario:** Taking a photo of the stadium but wearing glasses that only let you see people's faces (hiding their bodies). 
- **What it does:** Extracts specific **columns** from a table and discards the rest.
- **Example:** $\pi_{\text{Name, Email}}(\text{Students})$

### 3. Union ($\cup$) -> The Merger
- **Scenario:** Taking a list of people in the Chess Club, and a list of people in the Math Club, and merging them into one giant guest list. (Duplicates removed).
- **What it does:** Combines rows of two tables. *Both tables must have the exact same columns.*

### 4. Set Difference ($-$) -> The Excluder
- **Scenario:** Give me the guest list of the Chess club, MINUS anyone who is also in the Math club.
- **What it does:** Returns rows in Table A that are NOT in Table B.

### 5. Cartesian Product ($\times$) -> The Combiner (Cross Join)
- **Scenario:** If Table A is Shirts (Red, Blue) and Table B is Pants (Jeans, Khakis), Cartesian product creates every outfit combination: (Red+Jeans, Red+Khakis, Blue+Jeans, Blue+Khakis).
- **What it does:** Combines every row of Table A with every row of Table B.

---

## 🧩 The Derived Operator: JOIN ($\bowtie$)
- **What it does:** A Cartesian Product ($\times$) followed immediately by a Selection ($\sigma$). Instead of combining *everything*, it only combines rows where the keys match (e.g., `Employee.DeptID = Department.DeptID`).

## 🧠 Memorization Cheat Sheet
- **Selection ($\sigma$):** Filters Rows (Horizontal slice)
- **Projection ($\pi$):** Filters Columns (Vertical slice)
- **Union ($\cup$):** Add two lists together (Stacking Data)
- **Cross Product ($\times$):** Every possible combination
