# 05 — DevOps & Cloud

---

## Q9. Docker and Kubernetes

### 🌍 Real-World Application: Zomato's Consistent Kitchen

Imagine hiring 500 cooks for Zomato's corporate kitchen. If each cook brings their own pots, pans, and spices, the food will be wildly inconsistent. The solution: give EVERY cook a **standardized, pre-packaged kitchen kit**. That kit is **Docker**.

**docker = a standardized, self-contained box for your app.**
Inside the box: the code, the exact version of Node.js, the exact version of all libraries, the environment variables. It RUNS THE SAME whether it's on a developer's Windows laptop, a QA server running Ubuntu, or a production AWS machine.

**The Classic "Works on my machine" Problem — Solved.**

**Kubernetes = The Chef Manager who manages 500 cooks.**
Kubernetes decides:
- Which server to run each Docker container on.
- If a container crashes at 3am, it automatically starts a new one. (Self-healing)
- During dinner rush (peak traffic), it automatically spawns 20 more containers. During 3am, it scales back to 2. (Auto-scaling)

```
[Your Request] → [Kubernetes loads Load Balancer]
                         ↓
             [3 Identical Zomato App Containers]
             Container 1  Container 2  Container 3
```

### 💡 How to Answer
> *"Docker gives us environment consistency — a container built in development runs identically in production, eliminating an entire class of 'it works on my machine' bugs. I write multi-stage Dockerfiles to minimize final image sizes and always run containers as non-root users for security. In production, Kubernetes orchestrates these containers — I write deployment manifests defining resource limits, liveness and readiness probes, and HPA (Horizontal Pod Autoscaler) rules. I've managed K8s clusters on EKS where we auto-scaled from 10 to 80 pods during peak hours based on CPU metrics, then scaled back automatically."*

---

## Q10. Version Control with Git

### 🌍 Real-World Application: How Spotify Ships Features Without Chaos
**Spotify** has ~2,000 developers committing code daily. Without rigid version control practices, it would be pure chaos. Here's their Git workflow:

```
main (always production-stable)
├── develop (integration branch)
│   ├── feature/podcast-recommendations  (Alice's work)
│   ├── feature/social-listening-party   (Bob's work)
│   └── bugfix/playback-stutter          (Charlie's hotfix)
```

- Alice and Bob develop on separate branches simultaneously — no conflicts.
- Each branch goes through a **Pull Request (PR)** → peer code review → automated CI tests → then merges to `develop`.
- `develop` is merged to `main` only during a scheduled release window.
- If a bug is critical in production, a `hotfix/` branch is cut directly from `main`, fixed, and merged back to BOTH `main` and `develop`.

**GitOps:** Modern companies like Spotify also use GitOps — the Git repository is the single source of truth not just for code but for infrastructure. Merging to main AUTOMATICALLY deploys via the CI/CD pipeline.

### 💡 How to Answer
> *"I follow trunk-based development or Gitflow depending on team size — for larger teams, Gitflow provides better release management. Every feature is a short-lived branch, and I enforce a strict PR review process with at least one peer review and passing automated tests before any merge to the main branch. I use commit message conventions (Conventional Commits) that enable automatic changelog generation. I've also implemented Git hooks to prevent secrets from being committed and to enforce linting before commit."*

---

## Q16. Cloud Platforms — AWS, Azure, GCP

### 🌍 Real-World Application: OLA's AWS Architecture

**OLA Cabs** runs almost entirely on **AWS**. Here's how a single ride works on AWS:

| User Action | AWS Service Used | Why |
|---|---|---|
| Driver location sent every 3s | **DynamoDB** | Key-value, extremely fast writes |
| Match rider to driver | **EC2** Auto Scaling Group | Compute-heavy matching algorithm |
| Booking notification | **SNS + SQS** | Push notification fan-out |
| Trip invoice PDF | **Lambda** | Serverless, one-off computation |
| Payment processing | **RDS (Aurora)** | ACID transactions, critical |
| Driver profile photos | **S3** | Cheap object storage |
| CDN for app assets | **CloudFront** | Serve JS/CSS from edge globally |

### 💡 How to Answer
> *"I have extensive experience on AWS — I've designed systems using EC2 with Auto Scaling for compute, RDS Aurora for relational data with Multi-AZ failover for high availability, S3 for object storage, and ElastiCache (Redis) for caching. I've built serverless pipelines using Lambda + SQS + SNS for event-driven workloads. I also have hands-on experience with IaC (Infrastructure as Code) using Terraform, which means our entire infrastructure is version-controlled and reproducible. On Azure I've implemented Azure App Service with Azure SQL Database for enterprise clients."*

---

## Q19. CI/CD Pipelines

### 🌍 Real-World Application: How Swiggy Deploys 20+ Times a Day

**Swiggy** deploys new code to production multiple times a day without users ever noticing downtime. This is their CI/CD pipeline:

```
Developer pushes code
         ↓
    [GitHub / GitLab]
         ↓
    [CI Pipeline - Jenkins/GitHub Actions]
    ├── Run unit tests (must pass 100%)
    ├── Run integration tests
    ├── Security scan (SAST + dependency check)
    ├── Build Docker image
    └── Push to container registry
         ↓
    [CD Pipeline]
    ├── Deploy to Staging environment (automatic)
    ├── Run smoke tests on staging
    ├── Wait for manual approval (for production) OR automatic deploy
    └── Rolling deploy to Production (5% traffic → 25% → 100%)
         ↓
    [Monitor dashboards — 15 min bake time]
    └── Auto-rollback if error rate spikes
```

The magic: a developer merges code at 2pm and it's live on Swiggy.in by 2:15pm — automatically, safely.

### 💡 How to Answer
> *"CI/CD is non-negotiable for me in any serious backend team — it's the foundation of engineering velocity and quality. I've set up pipelines on Jenkins and GitHub Actions that enforce: all unit tests passing, code coverage above 80%, SonarQube quality gate passing, and zero critical CVEs before any artifact is built. On the CD side, I implement blue-green or canary deployments where 5% of traffic goes to the new version first. If error rates stay below threshold over 15 minutes, automated promotion deploys to 100%. Failed deployments trigger an automatic rollback to the previous stable image."*
