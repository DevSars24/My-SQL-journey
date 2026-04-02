# 06 — Code Quality: Refactoring, Logging, Dependencies, Testing

---

## Q7. Refactoring Legacy Code

### 🌍 Real-World Application: IRCTC's System Modernization
**IRCTC** (India's railway booking system) is infamous for crashing during Tatkal bookings. Its legacy code was written in the early 2000s — a classic monolith with tightly coupled modules, no tests, and shared mutable state. This is what refactoring legacy code looks like in real life.

**The Right Approach (Not "Rewrite Everything"):**
Senior engineers who've survived legacy code refactoring swear by the **Strangler Fig Pattern**:
1. Don't touch the old system. Keep it running.
2. Build new functionality alongside the old system.
3. Gradually "strangle" the old code by routing traffic to the new system piece by piece.
4. Once a module is fully migrated and tested, delete the old version.

**IRCTC's modernization steps:**
1. First, write tests for the existing behavior (even if ugly). Tests = a safety net.
2. Identify the highest-pain module (e.g., seat allocation). Extract it.
3. Refactor extracted module using SOLID principles. Inject dependencies. Make it testable.
4. Deploy behind a feature flag. Test with 1% traffic.
5. Gradually remove the old seat allocation module.

### 💡 How to Answer
> *"Refactoring legacy code is one of the highest-risk activities in software engineering, so I approach it with surgical precision. I begin by building a characterization test suite — tests that capture the CURRENT (even buggy) behavior, so I know immediately if my refactoring changes behavior unintentionally. I use the Strangler Fig pattern for large-scale migrations, extracting modules incrementally rather than big-bang rewrites. Every refactoring step is validated by the test suite. I also advocate for small, frequent refactoring commits over one massive refactoring PR that's impossible to review."*

---

## Q11. Error Handling and Logging

### 🌍 Real-World Application: How Uber Debugs a Failed Ride

When an **Uber** driver's app crashes during a ride, how does Uber's engineering team find out and why it happened — even before the driver calls support?

**Structured Logging (Every log line is a JSON object):**
```json
{
  "timestamp": "2024-01-15T14:32:01Z",
  "level": "ERROR",
  "service": "driver-location-service",
  "ride_id": "ride_abc123",
  "driver_id": "drv_456",
  "error": "GPS_TIMEOUT",
  "message": "Driver location update timed out after 5000ms",
  "region": "Mumbai",
  "trace_id": "trace_789xyz"
}
```

Why JSON? Because **Datadog, Splunk, or ELK Stack** can ingest these, allowing engineers to query: `level:ERROR AND service:driver-location-service AND region:Mumbai` and immediately see all errors in Mumbai in seconds.

**Log Levels (Use them correctly!):**
| Level | When to Use | Example |
|---|---|---|
| `DEBUG` | During development only | "Entering function calculateFare()" |
| `INFO` | Important business events | "Ride completed, fare = ₹250" |
| `WARN` | Non-critical, worth watching | "API response time = 4.8s (threshold: 5s)" |
| `ERROR` | Something broke, needs attention | "Payment gateway timed out, retry 3/3" |
| `CRITICAL` | System is down, wake someone up | "Database connection pool exhausted" |

**Centralized Alerting:** Uber's engineers are paged via PagerDuty the instant the error rate for `driver-location-service` exceeds 1% — automatically, no human monitoring dashboard required.

### 💡 How to Answer
> *"My logging philosophy: logs are your black box flight recorder. I always use structured JSON logging so logs are machine-queryable in tools like Datadog or ELK. I log at appropriate levels — never log sensitive PII (no passwords, no card numbers), always include a correlation/trace ID so I can trace a single request across 10 microservices. For error handling, I use custom exception hierarchies so `ValidationException`, `AuthException`, and `DatabaseException` can each be handled differently and mapped to the correct HTTP status codes. All errors feed into centralized monitoring with alerting thresholds for on-call response."*

---

## Q12. Managing Dependencies

### 🌍 Real-World Application: The npm Ecosystem and Why WhatsApp Cares
In 2016, a developer deleted a tiny npm package called `left-pad` (17 lines of code). This **broke thousands of projects worldwide** including Babel and React. This is the **dependency hell** problem.

**Best Practices (How mature teams handle this):**
1. **Lock File is Sacred:** `package-lock.json` or `yarn.lock` pins EXACT versions. Never commit `package.json` changes without a lock file.
2. **Semantic Versioning:** `^1.2.3` means "I accept 1.x.x changes but NOT 2.0.0 (breaking changes)".
3. **Automated Security Auditing:** `npm audit` in your CI pipeline automatically fails the build if a library has a CRITICAL vulnerability.
4. **Minimize Dependencies:** Every dependency is a liability. Don't add a 50KB library to do what 5 lines of code can do.
5. **Dependabot/Renovate Bot:** GitHub's Dependabot automatically opens PRs to update outdated dependencies weekly — so you're never 2 years behind.

### 💡 How to Answer
> *"Dependency management is a security and stability concern as much as a convenience one. I enforce lock files in all projects so builds are reproducible. I run automated `npm audit` and `pip-audit` in CI that blocks merges if critical CVEs are found — this is automatic, not optional. I keep dependencies minimal and intentional, and I review transitive dependencies when adding new packages. I've also set up Dependabot in all my repositories to get weekly automated PRs for dependency updates, making updates small and frequent rather than one terrifying big-bang upgrade."*

---

## Q14. Testing and Quality Assurance

### 🌍 Real-World Application: The Testing Pyramid at Amazon
Amazon deploys code every **11.7 seconds** (that's not a typo). This is only possible because of a rock-solid testing strategy:

**The Testing Pyramid:**
```
              /\
             /  \
            / E2E \       ← Few, slow, expensive, catch user-journey bugs
           /  Tests \
          /──────────\
         / Integration\   ← Medium, test service boundaries
        /    Tests     \
       /────────────────\
      /    Unit Tests    \  ← Many, fast, cheap, test pure logic
     /────────────────────\
```

**What each level tests for Amazon:**
- **Unit Tests:** "Does `calculateShippingCost(weight, distance)` return the correct value for all edge cases?" (Runs in milliseconds, 10,000 tests in 2 minutes.)
- **Integration Tests:** "Does the checkout service correctly talk to the payment service and inventory service?" (Runs in minutes.)
- **E2E Tests:** "Can a user search a product, add to cart, checkout, and receive a confirmation email?" (Runs in hours, usually overnight.)

**Test-Driven Development (TDD):** Write the test FIRST, watch it fail, then write the code to make it pass. This forces you to think about edge cases before writing implementation.

### 💡 How to Answer
> *"I follow the testing pyramid — the foundation is comprehensive unit tests (I practice TDD for critical domain logic), a middle layer of integration tests that test real database interactions and API contracts using tools like TestContainers, and a lean suite of E2E tests for critical user journeys. I enforce minimum 80% code coverage as a CI gate — not as a vanity metric but as a forcing function to write testable, modular code. I also implement contract testing using Pact for microservices to catch API breaking changes before they reach production."*
