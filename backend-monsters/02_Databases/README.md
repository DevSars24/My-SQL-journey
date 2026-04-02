# 02 — Databases: SQL vs NoSQL, Optimization & Migration

---

## Q4. SQL vs NoSQL — Experience and Preference

### 🌍 Real-World Application: Instagram's Dual Database Strategy
**Instagram** is one of the best real-world examples of using BOTH databases wisely:

| What Instagram Stores | Database Choice | Why |
|---|---|---|
| User accounts, follow relationships | **PostgreSQL (SQL)** | Following/Followers need complex JOIN queries. ACID guarantees prevent race conditions when two users follow each other simultaneously. |
| User activity feed, "liked by X and Y" | **Cassandra (NoSQL)** | Millions of writes per second. The feed just needs to be fast — not 100% perfectly consistent. |
| Session data, rate limiting counters | **Redis (NoSQL, in-memory)** | Must be microsecond-fast. A cache. 

The moment you post a photo on Instagram: SQL writes your post metadata; Cassandra fans out that event to all your followers' feeds; Redis checks your API rate limit in microseconds.

### 💡 How to Answer
> *"My preference isn't fixed — it's situational. For any data that requires strong relationships, complex queries, and strict ACID guarantees (like financial ledgers or user management), I reach for PostgreSQL. For data that requires extreme scale, high write throughput, and flexible schemas — like activity feeds, logs, or telemetry — I use MongoDB or Cassandra. In most mature systems I've worked on, we used both, each for what they do best. The anti-pattern I always warn against is using a NoSQL database just because it's 'modern' when a relational schema would fit the domain perfectly."*

---

## Q6. Database Performance Optimization Strategies

### 🌍 Real-World Application: Paytm's Transaction Speed
When you pay someone on **Paytm**, the confirmation appears in ~1 second. Behind the scenes:

1. **Indexing:** The `transactions` table has an index on `user_id` and `created_at`. Without an index, finding all YOUR transactions would scan millions of rows. With an index, it jumps directly.
2. **Query Analysis:** Engineers run `EXPLAIN ANALYZE` on slow queries to see if a full table scan is happening instead of an index seek.
3. **Caching:** Your wallet balance is cached in Redis. It doesn't query the database every time you open the app.
4. **Connection Pooling:** Instead of opening a new DB connection every request, Paytm reuses a pool of warm connections — saving hundreds of milliseconds.
5. **Read Replicas:** All payment reads go to a read replica, leaving the primary write database free for the critical payment commits.

### 💡 How to Answer
> *"My optimization strategy starts BEFORE writing code: I normalize the schema properly to avoid redundant data, then add indexes on columns that are frequently used in WHERE clauses and JOINs. In production, I monitor using slow query logs and `EXPLAIN`. I implement Redis caching for frequently read, rarely changed data. For high-traffic read endpoints, I route queries to read replicas, keeping the primary DB exclusively for writes. Connection pooling via PgBouncer ensures we don't exhaust DB connections under load."*

---

## Q20. Data Migration Between Systems

### 🌍 Real-World Application: A Bank Migrating Core Systems
A real-world nightmare: a bank migrating from an Oracle database to PostgreSQL. **Zero tolerance for data loss.**

**The ETL Approach:**
1. **Extract:** Read all customer data from Oracle.
2. **Transform:** Convert datatypes (Oracle's `NUMBER(19,4)` → PostgreSQL's `DECIMAL(19,4)`), clean nulls, map old schema to new.
3. **Load:** Write transformed data into PostgreSQL.
4. **Validate:** Row counts match? Checksums match? Run reconciliation reports comparing source and target.
5. **Cutover:** Switch over 2am Sunday night. Keep old DB running as a hot-standby for 7 days as fallback.

### 💡 How to Answer
> *"Data migration is a risk management exercise above all else. I begin by thoroughly mapping source to target schemas, identifying data type mismatches and nullability differences. I use ETL tools for transformation. Critically, I NEVER perform a big-bang migration — I run dual write strategies where possible, writing to both old and new systems simultaneously and reconciling data before the final cutover. Every migration I've led included automated rollback procedures and a clearly defined go/no-go checklist so stakeholders can make an informed decision on cutover day."*
