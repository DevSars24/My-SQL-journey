# 4. Normalization

## 📖 The Core Concept
**Normalization** is like Marie Kondo organizing a messy closet. 
Imagine a spreadsheet where an employee's address is typed out 50 times because they worked on 50 different projects. If they move, you have to update 50 rows! This is called an **Anomaly** (a glitch in the matrix).

- **Insertion Anomaly:** Can't add a new employee unless they are assigned a project.
- **Deletion Anomaly:** Deleting a project accidentally deletes the only record we have of an employee.
- **Updation Anomaly:** Updating an address in 49 rows but missing the 50th one, leading to inconsistent data.

Normalization breaks big messy tables down into smaller, focused tables to fix this.

---

## 🧹 The Normal Forms (The Cleaning Steps)

### 1st Normal Form (1NF): "No Multi-Valued Attributes"
- **The Mess:** Cell A2 contains `Math, Science, English`. 
- **The Fix:** A single cell can only hold a single atomic value. We must split this into 3 separate rows.
- **Scenario:** A spreadsheet cell can't hold a list or array. Flat is better!

### 2nd Normal Form (2NF): "No Partial Dependencies"
- **The Rule:** It must be in 1NF **AND** all non-key columns must depend on the **ENTIRE** primary key, not just a part of it.
- **The Mess:** Suppose primary key is `(StudentID, CourseID)`. We have a column called `Professor_Name`. `Professor_Name` depends ONLY on `CourseID`, not on the `StudentID`. 
- **The Fix:** Take `Professor_Name` and put it in a separate `Courses` table! Keep things related to the student in the student table, and course info in the course table.

### 3rd Normal Form (3NF): "No Transitive Dependencies"
- **The Rule:** It must be in 2NF **AND** non-key columns cannot depend on other non-key columns. (Everything must depend directly on the Primary Key, and nothing but the Primary Key).
- **The Mess:** `Employee_ID` -> `Zip_Code` -> `State`. Because `State` is determined by `Zip_Code`, it causes redundancy.
- **The Fix:** Create a separate `Zip_Codes` table mapping Zip codes to States. 

### Boyce-Codd Normal Form (BCNF): "The Stricter 3NF"
- **The Rule:** "For every dependency `A -> B`, `A` MUST be a Super Key."
- **Scenario:** It handles rare edge cases where 3NF still leaves slight redundancy, usually involving overlapping candidate keys.

---

## 🧠 Memorization Cheat Sheet
- **1NF:** Single Values Only (No lists in a cell).
- **2NF:** No Partial Dependencies (Whole key or leave).
- **3NF:** No Transitive Dependencies (No friends of friends).
- **Remember the mantra:** "Every non key attribute must provide a fact about the key (1NF), the whole key (2NF), and nothing but the key (3NF)."
