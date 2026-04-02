# 2. Entity-Relationship (ER) Model

## 📖 The Core Concept
Think of the **ER Model** as the **Architectural Blueprint** before building a house. Before writing a single line of SQL or creating tables, we draw a map of what data we need and how they relate.

## 🧱 The Building Blocks (Scenario: A University)

### 1. Entity (The Noun)
- **Scenario:** A `Student`, a `Professor`, or a `Course`. 
- **Definition:** A real-world distinguishable object. In diagrams, it's drawn as a **Rectangle**.

### 2. Attribute (The Adjective)
- **Scenario:** A Student's `Name`, `Age`, and `Student_ID`.
- **Definition:** Properties that describe the entity. In diagrams, drawn as **Ovals**.
- **Types of Attributes:**
  - **Key Attribute:** `Student_ID` (Underlined, unique for everyone).
  - **Composite Attribute:** `Address` (Can be broken down into Street, City, Zip).
  - **Multivalued Attribute:** `Phone_Number` (A student can have 2 phones. Drawn as a *Double Oval*).
  - **Derived Attribute:** `Age` (Calculated from Date of Birth. Drawn as a *Dashed Oval*).

### 3. Relationship (The Verb)
- **Scenario:** A Student **Enrolls** in a Course.
- **Definition:** How two entities interact. Drawn as a **Diamond**.

---

## 🔗 Types of Relationships (Cardinality)

### 1. One-to-One (1:1)
- **Scenario:** **One User** has **One Profile**. You can't have 2 profiles for exactly 1 account setup.
- **Rule:** A single entity in A relates to only one entity in B.

### 2. One-to-Many (1:N)
- **Scenario:** **One Mother** can have **Many Children**, but a child only has one biological mother.
- **Rule:** An entity in A can relate to multiple entities in B, but B only relates to one A.

### 3. Many-to-Many (M:N)
- **Scenario:** **Many Students** enroll in **Many Courses**. (Alice takes Math and Science; Math is taken by Alice and Bob).
- **Rule:** Entities on both sides can relate to multiple entities.

---

## 🧠 Memorization Cheat Sheet
- **Entity** = Noun (Rectangle)
- **Attribute** = Adjective (Oval)
- **Relationship** = Verb (Diamond)
- **1:1** = Driver & License
- **1:N** = Customer & Orders
- **M:N** = Students & Classes
