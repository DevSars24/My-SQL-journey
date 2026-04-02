# 01 — Backend Languages & RESTful API Design

---

## Q1. Describe your experience with backend programming languages (Java, Python, Ruby, etc.)

### 🔍 What the Interviewer Really Wants to Know
They are NOT asking you to list languages on a resume. They want to know:
- **Do you understand WHY one language over another?**
- **Can you make informed decisions for the right job?**

### 🌍 Real-World Application: Why Swiggy Uses Multiple Languages
When you open **Swiggy** and order food, several backend services are running simultaneously:
- The **Order Processing service** (needs high reliability, concurrency) → likely built with **Java/Spring Boot** because the JVM excels at managing thousands of concurrent requests safely.
- The **Image Resizer** (restaurant photos) → might use **Python** because Python has excellent image processing libraries like `Pillow`.
- The **Recommendation Engine** ("Trending near you") → definitely **Python** because of ML libraries like `scikit-learn` and `TensorFlow`.
- A rapid internal admin tool → potentially **Ruby on Rails** for its "convention over configuration" speed of development.

**The key insight: every language has a domain where it shines brightest.**

### 💡 How to Answer (STAR Method)
> *"Throughout my career I have primarily worked with Java for high-throughput services using Spring Boot, building RESTful APIs that handled millions of requests daily in a fintech context. I've also used Python explicitly for data processing pipelines — using Pandas to transform batch data before loading it into our warehouse. I choose the language based on the requirements: Java for concurrency & enterprise reliability, Python for data-heavy or ML workloads. I believe a senior developer's value is knowing WHEN to use which tool, not just knowing all tools equally."*

---

## Q3. How do you design and implement a RESTful API?

### 🔍 What & Why
REST (Representational State Transfer) is the lingua franca of the internet. It is why your phone can talk to Amazon's server, Paytm's server, and Zomato's server all using the same pattern.

### 🌍 Real-World Application: Zomato's Menu API
When you open a restaurant on **Zomato**, your phone sends:
```
GET /api/v1/restaurants/42/menu
```
Zomato's backend receives this and returns JSON with all the dishes. Notice **every design decision here is deliberate**:
- `GET` → you are **reading** data, not changing it. Safe and idempotent.
- `/v1/` → the API is **versioned**. If Zomato releases v2 of their API, old apps won't break.
- `/restaurants/42/menu` → the URL reads like English. It's a **resource-first** design. Not `/getMenuForRestaurant?id=42`.

**The 4 HTTP Verbs on Zomato:**
| Action | HTTP Method | Endpoint |
|---|---|---|
| View menu | `GET` | `/restaurants/42/menu` |
| Place an order | `POST` | `/orders` |
| Update cart quantity | `PUT` | `/orders/99/items/1` |
| Cancel order | `DELETE` | `/orders/99` |

### 💡 How to Answer
> *"I start by identifying my domain resources — e.g., `users`, `orders`, `products`. Then I map CRUD operations to HTTP verbs and design noun-based, hierarchical endpoints. I version the API from day one (`/v1/`) to enable future evolution without breaking existing clients. I enforce authentication (JWT Bearer token), validate all inputs, return standardized error objects with proper HTTP status codes (400 for validation errors, 401 for auth failures, 500 for server errors), and document everything in Swagger/OpenAPI so any frontend developer can onboard themselves."*
