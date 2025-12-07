# File 2: Types of Databases (Explained in Detail)

Databases store, manage, and organize data so applications can use it efficiently. Below are the **major types of databases**, each used for different scenarios.

---

# **1. Relational Databases (RDBMS)**

These store data in **tables (rows + columns)**.
Examples: **MySQL, PostgreSQL, Oracle, SQL Server, MariaDB**.

### **Characteristics**

* Use **SQL** (Structured Query Language)
* Data stored in **tables**
* Supports **relationships** using *foreign keys*
* Ensures **ACID properties** (safe, consistent data)

### **Best for:**

* Financial apps
* E-commerce
* SaaS applications
* Banking systems

---

# **2. NoSQL Databases**

These do **not use tables**. Designed for flexibility and high speed.
Examples: **MongoDB, Cassandra, DynamoDB, Redis**.

### **Types of NoSQL**

* **Document DB** → MongoDB
* **Key-value DB** → Redis
* **Column-based DB** → Cassandra
* **Graph DB** → Neo4j

### **Best for:**

* Real-time apps
* Flexible schema (JSON-based)
* High scalability

---

# **3. Cloud Databases**

Databases managed by cloud providers.
Examples: **Supabase (Postgres), Firebase, AWS RDS, PlanetScale**.

### **Benefits**

* Auto backup
* Auto scaling
* No manual server management

---

# **4. Distributed Databases**

Data stored across multiple systems but works like one.

### Used for:

* Large scale apps
* Big data processing

Examples: **CockroachDB, Cassandra**

---

# **5. Graph Databases**

Used to store **nodes and relationships**.

### Best For:

* Social networks
* Recommendation systems

Examples: **Neo4j, Amazon Neptune**

---

# **6. In-memory Databases**

Data stored temporarily in RAM.

### Best For:

* High-speed caching
* Session storage

Examples: **Redis, Memcached**
