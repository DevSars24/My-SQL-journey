# 07 — Performance, Caching & Message Brokers

---

## Q15. Troubleshooting Performance Issues in Production

### 🌍 Real-World Application: BookMyShow on a Coldplay Concert Drop
When **Coldplay India tickets** dropped on BookMyShow, the site crashed under the load. This is a real scenario backend engineers are fired for — or promoted for solving. Here's how a senior engineer investigates production performance issues:

**Step 1: Look at the Metrics First (Don't Guess)**
Before touching any code, go to Datadog/Grafana:
- CPU at 98%? Compute problem.
- DB query latency at 15 seconds? Database bottleneck.
- Memory increasing every minute? Memory leak.
- Network I/O maxed out? Bandwidth problem.

**Step 2: Read the Error Logs**
Search for `level:ERROR` in the time window the problem started. Often you find: `"Connection pool exhausted"` → database is the bottleneck.

**Step 3: Profile the Code (Find the Hot Path)**
Use a profiler (Java's VisualVM, Node's `--inspect`, Python's `cProfile`) to find which function consumes 90% of the CPU time. Often it's one N+1 query loop.

**The N+1 Problem (BookMyShow's Likely Culprit):**
```python
# BAD: This fires 1001 database queries!
events = db.query("SELECT * FROM events")  # 1 query
for event in events:  # 1000 events
    event.venue = db.query("SELECT * FROM venues WHERE id = ?", event.venue_id)  # +1000 queries
```
Fix: Use a single JOIN or use ORM eager loading.

**Step 4: Fix → Deploy → Monitor**
Fix the query, deploy, watch the metrics. Don't deploy on Friday evening.

### 💡 How to Answer
> *"My first instinct in a production incident is to diagnose before changing anything. I check dashboards for anomalies — CPU, memory, DB connection counts, and error rates — to form a hypothesis before touching code. I've resolved production performance issues by identifying N+1 query patterns using slow query logs, adding missing database indexes, tuning connection pool sizes, and adding strategic caching layers. Post-incident, I always write a blameless post-mortem documenting root cause, timeline, fix, and action items to prevent recurrence."*

---

## Q21. Caching — Importance and Implementation

### 🌍 Real-World Application: YouTube's Watch Count & Google Search

**Why does YouTube's "view count" sometimes show the same number for a while after a video goes viral?**

The view count is cached. YouTube doesn't update the database on EVERY single view in real-time — that would be millions of database writes per second for a viral video. Instead:
1. Views are batched and written to a **Redis counter** (in-memory, microsecond fast).
2. Every 30 seconds, the Redis count is flushed to the permanent database.
3. What you see is the cached count — slightly stale, but that's acceptable.

**Google Search's homepage** is another perfect example. The list of trending searches doesn't change every millisecond. It's cached for 5 minutes. ALL users see the same cached response, saving Google millions of database reads per hour.

**When to cache (and when NOT to):**
| Cache ✅ | Don't Cache ❌ |
|---|---|
| Product catalog (changes rarely) | Bank account balance (must be real-time) |
| Restaurant menu on Zomato | Live OTP codes |
| User profile data | Shopping cart quantities |
| Country/city list for dropdowns | Stock market prices |

**Cache Invalidation Strategies:**
- **TTL (Time-To-Live):** "Delete this cache entry after 5 minutes."
- **Write-Through:** Update cache AND database simultaneously on every write.
- **Cache-Aside (Lazy Loading):** Check cache first. Cache miss → query DB → store in cache → return. Cache hit → return directly.

### 💡 How to Answer
> *"Caching is one of the highest-ROI optimizations in backend development. I implement Redis as my caching layer for: frequently-read, rarely-changed data (product catalogs, reference data), aggregated computation results (leaderboards, analytics summaries), and session data. I use the Cache-Aside pattern for most cases and set TTLs that balance freshness with performance. A critical discipline I always practice: never cache sensitive PII or financial data, and always design cache invalidation before designing the cache strategy — because a stale cache is sometimes worse than no cache."*

---

## Q22. Message Brokers — Kafka and RabbitMQ

### 🌍 Real-World Application: PhonePe's Transaction Pipeline

Every UPI payment on **PhonePe** is not just "Deduct from A, add to B". It triggers a cascade of events:
- Send notification to sender.
- Send notification to receiver.
- Update loyalty rewards points.
- Log for fraud detection ML model.
- Update daily transaction analytics.

If all of this happened synchronously, your payment would take 5+ seconds. Instead, PhonePe uses **Apache Kafka**:

```
[Payment Confirmed]
        │
[Publish Event to Kafka Topic: "payment.completed"]
        │
    ┌───┴────────────────────────────────────┐
    ▼           ▼           ▼                ▼
[SMS Service] [Rewards] [Fraud ML Model] [Analytics]
  (reads &    (reads &     (reads &        (reads &
  processes)  processes)   processes)      processes)
```

Each service independently consumes from the Kafka topic at its own pace. The payment confirmation is instant. The notification might come 200ms later. That's totally fine.

**Kafka vs RabbitMQ — The Quick Comparison:**
| Feature | Kafka | RabbitMQ |
|---|---|---|
| **Best for** | High-throughput event streaming | Task queuing with complex routing |
| **Message retention** | Keeps messages for days/weeks (replayable) | Deletes after consumed |
| **Real-world use** | Uber's location streams, payment events | Email job queues, task processing |
| **Throughput** | Millions of msgs/sec | Hundreds of thousands/sec |

### 💡 How to Answer
> *"I've used both Kafka and RabbitMQ depending on the use case. For high-throughput event streaming where you need message replay capability — like audit logs, payment events, or IoT telemetry — Kafka is the right choice. Its persistent log model means if a downstream consumer goes down, it can replay from where it left off. For more traditional job queue patterns — where you want acknowledgment-based delivery, dead-letter queues for failed tasks, and complex routing logic — RabbitMQ is a better fit. I've implemented Kafka-backed pipelines where order events fan out to 5 different consumers: notifications, inventory, analytics, recommendations, and fraud detection — all decoupled and independently scaled."*
