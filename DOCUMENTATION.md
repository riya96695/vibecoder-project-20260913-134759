# Food‑Delivery Platform  
> **Monorepo** – Front‑end (Next.js + React), Back‑end (NestJS + GraphQL), Infra (Terraform/K8s)  
> **Production‑ready** – Docker/K8s, CI/CD, observability, security & compliance

---

## 📖 README

### 1. Overview
A full‑stack, cloud‑native food‑delivery platform that supports **customers, restaurants, drivers, and admins**.  
The system is built on a **monorepo** with a **NestJS** GraphQL API, **Next.js** front‑end, and **Terraform**‑managed AWS infra.  
It satisfies all functional & non‑functional requirements from the brief and is ready for rapid MVP delivery.

### 2. Features (MVP‑ready)

| # | Feature | MVP‑Scope |
|---|---------|-----------|
| FR‑001 | User Registration & Auth | Email/phone, social login, password reset |
| FR‑003 | Restaurant Profile | CRUD, logo, menu, categories, items |
| FR‑004 | Menu Browsing | Search, filter, pagination |
| FR‑005 | Cart | Persisted in localStorage |
| FR‑006 | Order Placement | Address, time, special instructions |
| FR‑007 | Payments | Stripe & PayPal |
| FR‑009 | Real‑Time Status | WebSocket updates |
| FR‑010 | Driver Assignment | Auto‑assign nearest driver, ETA |
| FR‑020 | Notification Service | Email/SMS via SendGrid/Twilio |

### 3. Architecture

```
┌───────────────────────────────────────────────────────────────────────┐
│ 1️⃣ API Gateway (ALB / NGINX)  ──► 2️⃣ Auth Service (Auth0)            │
│  │                                                    │
│  │  ┌───────────────────────┐  ┌───────────────────────┐ │
│  │  │  NestJS GraphQL API   │  │  Next.js Front‑end    │ │
│  │  │  (Docker/K8s)         │  │  (Docker/K8s)         │ │
│  │  └───────────────────────┘  └───────────────────────┘ │
│  │                                                    │
│  │  ┌───────────────────────┐  ┌───────────────────────┐ │
│  │  │  PostgreSQL (RDS)     │  │  Redis (ElastiCache)  │ │
│  │  │  MongoDB (optional)   │  │  Pub/Sub (Socket.io)  │ │
│  │  └───────────────────────┘  └───────────────────────┘ │
│  │                                                    │
│  │  ┌───────────────────────┐  ┌───────────────────────┐ │
│  │  │  Stripe / PayPal      │  │  SendGrid / Twilio    │ │
│  │  └───────────────────────┘  └───────────────────────┘ │
└───────────────────────────────────────────────────────────────────────┘
```

### 4. Tech Stack

| Layer | Technology | Why |
|-------|------------|-----|
| Front‑end | **Next.js** + **React** + **TypeScript** + **Tailwind CSS** | SSR, SEO, component reuse |
| API | **NestJS** + **GraphQL** + **TypeScript** | Structured, modular, code‑first |
| DB | **PostgreSQL** (primary) + **Redis** | ACID, caching, pub/sub |
| Real‑time | **Socket.io** | Order status & driver tracking |
| Payments | **Stripe** + **PayPal** | PCI‑DSS, global coverage |
| Auth | **Auth0** | OAuth2, MFA, social login |
| Infra | **AWS** (ECS/EKS, RDS, ElastiCache, S3, CloudFront) | Managed, scalable |
| CI/CD | **GitHub Actions** | Automated tests & deploy |
| Observability | **Prometheus** + **Grafana**, **ELK** | Metrics & logs |
| Testing | **Jest**, **Supertest**, **Cypress** | Unit, integration, E2E |

### 5. Prerequisites

| Tool | Minimum Version |
|------|-----------------|
| Node.js | 20.x |
| Yarn | 1.22.x |
| Docker | 20.x |
| Docker‑Compose | 1.29.x |
| Terraform | 1.6.x |
| AWS CLI | 2.x |
| Git | 2.30+ |

> **Tip** – Use **asdf** or **nvm** to manage Node versions.

### 6. Quick Start

```bash
# 1️⃣ Clone the repo
git clone https://github.com/your-org/food-delivery.git
cd food-delivery

# 2️⃣ Install dependencies (monorepo)
yarn install

# 3️⃣ Copy env templates
cp apps/backend/.env.example apps/backend/.env
cp apps/frontend/.env.example apps/frontend/.env
cp infra/.env.example infra/.env

# 4️⃣ Spin up dev stack
docker compose up -d

# 5️⃣ Run migrations & seed data
docker compose exec backend yarn db:migrate
docker compose exec backend yarn db:seed

# 6️⃣ Open the app
open http://localhost:3000
```

> The API is available at `http://localhost:4000/graphql`.  
> The admin UI (if any) at `http://localhost:4000/admin`.

### 7. Development

| Task | Command |
|------|---------|
| Start all services | `docker compose up -d` |
| Stop services | `docker compose down` |
| Run backend tests | `yarn workspace @food-delivery/backend test` |
| Run frontend tests | `yarn workspace @food-delivery/frontend test` |
| Run e2e tests | `yarn workspace @food-delivery/frontend cypress:run` |
| Build for production | `yarn workspace @food-delivery/frontend build` & `yarn workspace @food-delivery/backend build` |
| Lint | `yarn lint` |
| Format | `yarn format` |

### 8. Deployment

> **Assumptions** – You have an AWS account and the required IAM permissions.

```bash
# 1️⃣ Build Docker images
docker compose build

# 2️⃣ Push to ECR
aws ecr get-login-password | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
docker tag food-delivery-backend:latest <account-id>.dkr.ecr.<region>.amazonaws.com/food-delivery-backend:latest
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/food-delivery-backend:latest
# (repeat for frontend)

# 3️⃣ Deploy infra (Terraform)
cd infra
terraform init
terraform apply -auto-approve

# 4️⃣ Deploy to EKS (Helm or Kustomize)
helm upgrade --install food-delivery ./helm/food-delivery \
  --set image.repository=<account-id>.dkr.ecr.<region>.amazonaws.com/food-delivery-backend \
  --set image.tag=latest \
  --namespace food-delivery
```

> **CI/CD** – The repo contains GitHub Actions workflows (`ci.yml`, `cd.yml`) that automatically run tests, build images, and push to ECR on every PR merge.

### 9. Contributing

1. Fork & clone the repo.  
2. Create a feature branch (`git checkout -b feat/<short-name>`).  
3. Run `yarn lint && yarn format`.  
4. Write tests.  
5. Submit a PR.  
6. Follow the **Code of Conduct**.

### 10. License

MIT © 2026 Food‑Delivery Inc