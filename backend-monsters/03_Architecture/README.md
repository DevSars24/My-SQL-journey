# 03 — Architecture: Microservices, Serverless, Scalability

---

## Q8. Microservices Architecture and Its Benefits

### 🌍 Real-World Application: Swiggy's Order Flow
Think about what happens when you press "Place Order" on **Swiggy**:

```
[Your Phone]
     │
     ▼
[API Gateway] ──► [Order Service]       → Creates order
                   ──► [Restaurant Service]  → Notifies restaurant
                   ──► [Payment Service]     → Charges your UPI
                   ──► [Delivery Service]    → Assigns a delivery partner
                   ──► [Notification Service]→ Sends you a push notification
                   ──► [Analytics Service]   → Logs the event for data team
```

If Swiggy was a **monolith**, all of this would be one giant application. When they need to scale delivery matching (which is compute-heavy), they'd have to scale the ENTIRE app including the notification service (which barely uses any resources). Wasteful.

With **microservices**, they independently scale only the Delivery Service during peak dinner hours 7-9pm. The Notification Service stays small. Cost drops dramatically.

**Key Benefits in Swiggy's context:**
- **Independent Scaling:** Scale delivery matching without scaling payments.
- **Independent Deployments:** Fix a bug in the restaurant service WITHOUT deploying the payment service.
- **Fault Isolation:** If the Analytics service crashes, your payment still works. The whole app doesn't die.
- **Technology Freedom:** Payment Service (Java for ACID safety) vs. Analytics Service (Python for data processing).

### 💡 How to Answer
> *"Microservices architecture decomposes an application into small, independently deployable services each owning their own data and domain logic. The critical benefits I've experienced firsthand are: independent scalability (scale only what's under load), independent deployability (faster release cycles and reduced blast radius for bugs), and fault isolation. The tradeoff is operational complexity — you now need service discovery, distributed tracing, API gateways, and careful handling of inter-service failures. My rule of thumb: don't start with microservices. Start as a well-structured monolith, identify your pain points at scale, then extract services where the boundary is clear."*

---

## Q26. Serverless Architecture and Its Benefits

### 🌍 Real-World Application: MakeMyTrip's Email Confirmation
When you book a flight on **MakeMyTrip**, you immediately get a booking confirmation email. This email sender is a perfect serverless use case.

Without serverless: you'd run a dedicated server 24/7 waiting to send emails. But at 3am, nobody books flights. The server sits idle burning money.

With **AWS Lambda (serverless):**
1. Booking is confirmed → triggers an event.
2. Lambda function wakes up, runs for 500ms, sends the email, dies.
3. You pay for exactly 500ms. That's it.

**MakeMyTrip events that fit serverless perfectly:**
- Sending confirmation emails/SMS on booking.
- Resizing hotel photos to different screen sizes on upload.
- Generating monthly invoice PDFs.
- Processing periodic refund batches at midnight.

### 💡 How to Answer
> *"Serverless shines for event-driven, stateless, intermittent workloads. On a previous project, I used AWS Lambda to handle post-purchase events — sending confirmation emails, updating loyalty points, and triggering inventory reduction. Cost dropped significantly because we only paid for invocation time. The key limitations I keep in mind: cold starts can add latency (mitigated with Provisioned Concurrency for latency-sensitive paths), the 15-minute runtime limit means it's not suitable for long-running jobs, and debugging distributed serverless systems requires structured logging and X-Ray tracing."*

---

## Q27. Handling Large Amounts of Data — Scalability

### 🌍 Real-World Application: Hotstar During IPL
**Disney+ Hotstar** broke the world record for concurrent streaming viewers during IPL — 50 million simultaneous users in 2023. How?

- **Horizontal Scaling:** Not one giant server but thousands of small ones behind a load balancer. When viewership spikes at 6pm for the match start, new servers auto-spin up.
- **CDN (Content Delivery Network):** The video is cached at servers physically closest to YOU — not streamed from a US datacenter. Akamai/CloudFront serves you from Mumbai.
- **Database Sharding:** Your user data (region=Mumbai) goes to a different database shard than users from Chennai. No single DB is a bottleneck.
- **Async Processing:** Scorecards, comments, reactions → processed through message queues so the main video stream is never blocked by social features.

### 💡 How to Answer
> *"My approach to large-scale data systems starts with choosing the right architecture before writing a line of code. I design for horizontal scaling from day one — stateless services behind load balancers that can scale out. I use database sharding or partitioning to distribute write load, implement caching aggressively at the application, database, and CDN layers, and decouple heavy async workloads using message queues. I define SLOs and instrument everything with metrics/alerting so bottlenecks are visible before they become outages."*

---

## Q28. High Availability and Fault Tolerance

### 🌍 Real-World Application: Google Maps During New Year's Eve
**Google Maps** processes location data for hundreds of millions of people simultaneously. A failure at midnight New Year's Eve would be catastrophic.

- **Multi-region Deployment:** Maps servers run in US, Europe, Asia SIMULTANEOUSLY. If an AWS datacenter in Mumbai goes down, Indian users are seamlessly served from Singapore.
- **Health Checks + Auto-Healing:** Kubernetes continuously checks if pods are responsive. A failing pod is automatically killed and a new one starts.
- **Circuit Breakers:** If the traffic prediction service is slow, Maps serves results WITHOUT the traffic layer rather than timing out for the user.
- **Chaos Engineering (Chaos Monkey):** Netflix (and Google) intentionally kill random servers in production to ensure the system heals itself automatically!

### 💡 How to Answer
> *"High availability requires redundancy at every layer — multiple instances across availability zones, read replicas for databases, and active-active deployments across regions for truly critical services. For fault tolerance specifically, I implement circuit breakers (using libraries like Resilience4j) to stop cascading failures, timeouts on all inter-service calls, and graceful degradation — the system serves a reduced experience rather than failing completely. Health checks and automated failover are non-negotiable. I also advocate for chaos engineering — intentionally introducing failures in staging to build confidence in the recovery mechanisms."*
