# vibecoder-project-20260913-134759

## Original Idea
make food ordering website


## Requirements
## 1. Functional Requirements

| # | Feature | Description | Primary Actors | Notes |
|---|---------|-------------|----------------|-------|
| **FR‑001** | **User Registration & Authentication** | Users can create an account (email/phone) and log in. Password reset, email verification, social login (Google, Facebook). | Customer, Restaurant Owner, Delivery Driver, Admin | Use OAuth 2.0 / JWT for stateless auth. |
| **FR‑002** | **Role‑Based Access Control** | Different UI/permissions for Customer, Restaurant, Driver, Admin. | All | Implement RBAC in backend. |
| **FR‑003** | **Restaurant Profile Management** | Owners can add/edit restaurant details, logo, address, opening hours, menu categories, items, prices, images, availability. | Restaurant Owner | CRUD operations via REST/GraphQL. |
| **FR‑004** | **Menu Browsing & Search** | Customers can view menus, filter by cuisine, price, rating, dietary tags, search by keyword. | Customer | Pagination, infinite scroll. |
| **FR‑005** | **Cart & Order Composition** | Add items, modify quantity, apply modifiers (size, toppings), view subtotal, taxes, delivery fee. | Customer | Persist cart in session/localStorage. |
| **FR‑006** | **Order Placement** | Submit order with delivery address, time, special instructions, payment method. | Customer | Validate address via geocoding. |
| **FR‑007** | **Payment Integration** | Accept credit/debit cards, digital wallets, cash on delivery. Process via Stripe/PayPal/Apple Pay. | Customer | PCI‑DSS compliant. |
| **FR‑008** | **Order Confirmation & Receipt** | Email/SMS confirmation, order summary, estimated delivery time. | Customer, Restaurant | Use templated emails. |
| **FR‑009** | **Real‑Time Order Status** | Track order stages: received, preparing, out for delivery, delivered. Push updates via WebSocket or long polling. | Customer, Driver, Restaurant | Optional push notifications. |
| **FR‑010** | **Driver Assignment & Tracking** | Auto‑assign nearest driver, display ETA, GPS tracking on map. | Driver, Customer | Integration with Google Maps API. |
| **FR‑011** | **Ratings & Reviews** | Customers rate food, service, delivery. Restaurants can respond. | Customer, Restaurant | Moderation queue. |
| **FR‑012** | **Promotions & Coupons** | Create discount codes, time‑limited offers, loyalty points. | Admin, Restaurant | Apply to cart. |
| **FR‑013** | **Analytics Dashboard** | Sales, order volume, top dishes, driver performance. | Admin, Restaurant | Charts, export CSV. |
| **FR‑014** | **Admin Management** | Manage users, restaurants, drivers, content, site settings. | Admin | CRUD, bulk import/export. |
| **FR‑015** | **Multi‑Language & Currency** | Support locale‑specific languages, currencies. | All | i18n, l10n. |
| **FR‑016** | **Accessibility** | WCAG 2.1 AA compliance. | All | Screen reader support, keyboard navigation. |
| **FR‑017** | **Responsive Design** | Mobile, tablet, desktop. | All | Adaptive UI. |
| **FR‑018** | **Security Features** | CSRF protection, XSS sanitization, rate limiting, audit logs. | All | OWASP Top 10. |
| **FR‑019** | **Data Export** | Export orders, revenue, user data. | Admin, Restaurant | CSV/JSON. |
| **FR‑020** | **Email/SMS Notification Service** | Order updates, promotions, password resets. | All | Integration with SendGrid/Twilio. |

---

## 2. Non‑Functional Requirements

| # | Requirement | Description | Acceptance Criteria |
|---|-------------|-------------|---------------------|
| **NFR‑001** | **Scalability** | Handle thousands of concurrent users, orders, and real‑time updates. | Auto‑scaling on cloud, load‑balanced API tier. |
| **NFR‑002** | **Performance** | Page load < 2 s (desktop), < 3 s (mobile). Order placement < 1 s. | Lighthouse score ≥ 90. |
| **NFR‑003** | **Availability** | 99.9 % uptime (excluding scheduled maintenance). | SLA, uptime monitoring. |
| **NFR‑004** | **Security** | PCI‑DSS compliance for payments, GDPR/CCPA for user data. | Security audit, penetration test. |
| **NFR‑005** | **Reliability** | Order data durability, no data loss on crash. | Database replication, backup strategy. |
| **NFR‑006** | **Maintainability** | Modular code, clear API contracts, automated tests. | Code coverage ≥ 80 %. |
| **NFR‑007** | **Extensibility** | Easy addition of new payment methods, delivery partners. | Plugin architecture, clear SDK. |
| **NFR‑008** | **Usability** | Intuitive UI, minimal steps to order. | User testing, task success rate ≥ 95 %. |
| **NFR‑009** | **Accessibility** | WCAG 2.1 AA compliance. | Automated accessibility tests. |
| **NFR‑010** | **Internationalization** | Support at least 3 locales. | Locale switcher, proper formatting. |
| **NFR‑011** | **Compliance** | PCI‑DSS, GDPR, local food‑service regulations. | Legal review. |
| **NFR‑012** | **Logging & Monitoring** | Centralized logs, metrics, alerts. | Integration with ELK/Prometheus. |
| **NFR‑013** | **Backup & Disaster Recovery** | Daily backups, point‑in‑time recovery. | RPO < 15 min, RTO < 1 h. |
| **NFR‑014** | **API Rate Limiting** | Protect against abuse. | 1000 requests/min per IP. |
| **NFR‑015** | **Data Privacy** | Users can delete account, data export. | GDPR “right to be forgotten”. |

---

## 3. Recommended Technology Stack

| Layer | Suggested Tech | Rationale |
|-------|----------------|-----------|
| **Front‑End** | **React** (Next.js for SSR) + **TypeScript** | Component reusability, SEO, fast rendering. |
|  | **React‑Query** or **Apollo Client** | Efficient data fetching, caching. |
|  | **Tailwind CSS** or **Material‑UI** | Rapid styling, responsive design. |
| **Back‑End** | **Node.js** + **Express** (or NestJS) **+ TypeScript** | Async I/O, large ecosystem, easy integration with JS front‑end. |
|  | **GraphQL** (Apollo Server) | Flexible queries for mobile/desktop. |
| **Database** | **PostgreSQL** (primary) + **Redis** (caching, pub/sub) | ACID compliance, relational data for orders, caching for menu. |
|  | **MongoDB** (optional) | For unstructured data like reviews, logs. |
| **Real‑Time** | **Socket.io** or **Pusher** | Order status updates, driver tracking. |
| **Payments** | **Stripe** (cards, Apple Pay, Google Pay) + **PayPal** | PCI‑DSS, global coverage. |
| **Geolocation & Maps** | **Google Maps API** (Directions, Places) | Driver routing, address autocomplete. |
| **Notifications** | **Twilio** (SMS) + **SendGrid** (Email) | Reliable delivery, templates. |
| **Auth** | **Auth0** or **Firebase Auth** | OAuth, social login, MFA. |
| **Infrastructure** | **AWS** (EC2, ECS/EKS, RDS, ElastiCache, S3, CloudFront) | Scalability, managed services. |
|  | **Docker** + **Kubernetes** | Containerization, orchestration. |
|  | **CI/CD** (GitHub Actions / GitLab CI) | Automated tests, deployments. |
| **Monitoring** | **Prometheus** + **Grafana** | Metrics. |
|  | **ELK Stack** (Elasticsearch, Logstash, Kibana) | Logging. |
| **Testing** | **Jest** + **React Testing Library** (frontend) |
|  | **Supertest** + **Mocha** (backend) |
|  | **Cypress** |

## Architecture / Plan
Below is a **complete, production‑ready blueprint** for the food‑delivery platform you described.  
It covers the **architecture**, the **road‑map** to ship the MVP, the **database design**, the **module list** and a **folder layout** that will keep the codebase clean, testable and scalable.

> **TL;DR**  
> • **Monorepo** (frontend + backend + infra) with **Docker/K8s**.  
> • **NestJS + GraphQL** backend, **Next.js + React** frontend.  
> • **PostgreSQL** + **Redis** (cache & pub/sub).  
> • **Auth0** for OAuth2/JWT, **Stripe** + **PayPal** for payments.  
> • **Socket.io** for real‑time order status & driver tracking.  
> • **AWS** (ECS/EKS, RDS, ElastiCache, S3, CloudFront, Secrets Manager).  
> • CI/CD with **GitHub Actions** → **ECR** → **EKS**.  
> • Observability: **Prometheus + Grafana**, **ELK**.  
> • Testing: **Jest** (unit), **Supertest** (integration), **Cypress** (E2E).  

---

## 1. Project Architecture

```
┌───────────────────────────────────────────────────────────────────────┐
│                           System Overview                            │
├───────────────────────────────────────────────────────────────────────┤
│ 1️⃣  API Gateway (NGINX / ALB)  ──►  2️⃣  Auth Service (Auth0)      │
│
