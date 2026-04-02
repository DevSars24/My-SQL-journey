# 04 — Security & Authentication

---

## Q5. How Do You Ensure Code Is Secure and Free From Vulnerabilities?

### 🌍 Real-World Application: Why HDFC Bank Wasn't Hacked
Every day, millions of transactions flow through **HDFC Bank's** net banking portal. They survive because their engineers follow these exact same practices:

**1. Input Validation (The Bouncer at the Door)**
Imagine typing `'; DROP TABLE accounts;--` into a transfer amount field. If the bank's backend trusts user input and puts it directly into a SQL query — your accounts table is gone. This is an **SQL Injection** attack.

HDFC prevents this by:
- Using **Prepared Statements** with parameterized queries (the input is never executed as code).
- Validating that the "amount" field is a positive number BEFORE it reaches any database layer.

**2. Principle of Least Privilege (The Employee ID Card)**
A bank teller's employee account can open the vault room door but NOT the CEO's office. Similarly, the database user account for the public API can only `SELECT` from the accounts table — it has ZERO `DROP` or `ALTER` privileges.

**3. Dependency Auditing (The Expired Fire Extinguisher)**
In 2017, Equifax (a US credit bureau) was hacked because they were running a web framework (Apache Struts) with a KNOWN vulnerability that had a patch available for months. They didn't update it. 147 million people's data was stolen.

Solution: Use `npm audit`, `pip-audit`, or `OWASP Dependency Check` in your CI pipeline to automatically block deployments if a vulnerable library is detected.

**4. Rate Limiting (The Login Throttle)**
When someone tries to guess your password on **Google**, Google limits them to ~5 attempts, then locks the account. Without rate limiting, automated bots can try 10,000 password combinations per second.

### 💡 How to Answer
> *"Secure coding is not an afterthought for me — it's built into every step of the SDLC. I enforce input validation with whitelisting, use parameterized queries exclusively (no string concatenation in SQL), implement the principle of least privilege for all service accounts, and run automated OWASP dependency checks in CI that block merges if critical CVEs are found. I also enforce rate limiting on all public endpoints and require TLS 1.2+ for all service-to-service communication. I've also run annual security drills where our team performs internal penetration testing on our own APIs."*

---

## Q13. Authentication and Authorization Mechanisms

### 🌍 Real-World Application: How Netflix Knows It's YOU (and Not Your Neighbor)

Every time you open **Netflix**, two things happen invisibly:

**Authentication (Who are you?)**
You log in with email/password → Netflix verifies → issues you a **JWT (JSON Web Token)**.

A JWT looks like: `eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjEyMzQ1fQ.signature`

It contains (encoded): `{ userId: 12345, plan: "Premium", exp: "2024-01-01" }`

Netflix's backend doesn't need to look up a database session for EVERY request. It just cryptographically verifies the token's signature. This is why JWT enables **stateless authentication** — critical for Netflix's scale.

**Authorization (What can YOU do?)**
Netflix has roles:
- `free_user` → Can only watch SD content, no downloads.
- `premium_user` → Can watch 4K, download 25 videos.
- `admin` → Can access the content management dashboard.

When you try to watch a 4K movie, Netflix's middleware checks your JWT's plan field BEFORE serving you the 4K stream. This is **Role-Based Access Control (RBAC)**.

**The OAuth2 Flow (Login with Google/Facebook)**
When you click "Login with Google" on any app, you're using **OAuth2**:
1. You are redirected to Google's servers to log in.
2. Google gives the app a temporary "authorization code".
3. The app exchanges it for an access token.
4. The app NEVER sees your Google password. Google acts as the trusted identity provider.

### 💡 How to Answer
> *"I implement authentication using JWT for stateless REST APIs — the token carries the user's identity and is verified via asymmetric signature on every request without a database lookup. For authorization, I implement RBAC middleware that checks the user's role/permissions against what the specific endpoint requires, returning a 403 Forbidden if they lack permission. For third-party login (SSO), I implement OAuth2 with providers like Google using the PKCE flow for added security. I always set short JWT expiration times (15 minutes for access tokens) paired with longer-lived refresh tokens stored in HttpOnly cookies to prevent XSS theft."*
