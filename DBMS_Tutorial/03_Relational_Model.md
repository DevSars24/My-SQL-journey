# 3. Relational Model & Keys

## 📖 The Core Concept
If the ER Model is the architectural blueprint, the **Relational Model** is the actual physical house built with bricks. It organizes data into tables (Relations), rows (Tuples), and columns (Attributes).

---

## 🔑 The Key Players (Scenario: A High School Roster)

Imagine a table of students: `Roll_No`, `Email`, `Aadhar_Card_No` (National ID), `Name`, `Age`.

### 1. Super Key (The "I got you" Key)
- **Scenario:** If I uniquely want to find you in the school, what combination of info works? 
  `Roll_No` works. `(Roll_No, Name)` works. `(Email, Age)` works. 
- **Definition:** ANY combination of columns that uniquely identifies a row.

### 2. Candidate Key (The "Minimalist" Super Key)
- **Scenario:** We don't need `(Roll_No, Name)` to find you. `Roll_No` alone is enough. `Email` alone is enough. `Aadhar_Card_No` alone is enough.
- **Definition:** A Super Key with absolutely no extra/redundant columns. It's the bare minimum needed.

### 3. Primary Key (The "Chosen One")
- **Scenario:** The database administrator must choose ONE of the Candidate Keys to be the official identifier. They choose `Roll_No`.
- **Definition:** The single chosen Candidate Key used to identify rows. It cannot be NULL and must be strictly unique.

### 4. Alternate Key (The "Runners Up")
- **Scenario:** Since `Roll_No` was chosen as Primary, `Email` and `Aadhar_Card` are the losers. They are Alternative Keys.
- **Definition:** Any Candidate key that was NOT chosen as the Primary Key.

### 5. Foreign Key (The "Bridge")
- **Scenario:** In our `Grades` table, we don't rewrite the student's entire name, address, and email. We just drop in the `Roll_No` to point back to the `Students` table.
- **Definition:** A column in one table that references the Primary Key of another table. It enforces referential integrity.

---

## 🔗 Functional Dependencies (FD)
- **Scenario:** If I give you `Roll_No`, you can definitively tell me the `Name`. Therefore, `Name` is **functionally dependent** on `Roll_No`. Written as `Roll_No -> Name`.
- **Why it matters:** FDs are the foundation of Normalization. If `Age` can be derived from `DOB`, `DOB -> Age`.

---

## 🧠 Memorization Cheat Sheet
- **Super Key:** Anything that works (messy).
- **Candidate Key:** Anything that works *minimally* (clean).
- **Primary Key:** The single one we officially choose.
- **Alternate Key:** The candidate keys we didn't choose.
- **Foreign Key:** The link connecting two tables together.
