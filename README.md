# Hey, I'm Sammy 👋

Senior Full Stack Engineer based in Brazil, working remotely with US companies for 3+ years.
Java backends, React frontends, distributed systems at scale.

Currently at **Avangrid** — payment processing and billing platforms for 400K+ energy customers.

---

## What I'm building

### 🧠 [JobFlow AI](https://github.com/SamDev98/jobflow-ai)
Multi-tenant SaaS for job application management with AI-powered resume optimization and Kanban pipeline tracking.

- OpenAI-compatible AI integration via OpenRouter with Redis cache (TTL 7 days) — cuts token cost significantly
- Clerk JWT auth with per-tenant isolation via Spring Security
- Full CI/CD: Java backend on Fly.io + React frontend on Vercel
- Demo mode decoupled from backend — runs frontend-only with mock data for portfolio

**Stack:** Java 21 · Spring Boot 3.5 · React 18 · TypeScript · PostgreSQL · Redis · Docker · Clerk

---

### ⚔️ [RPG Master AI](https://github.com/SamDev98/rpg-master-ai)
Production-grade RAG system that ingests RPG rulebooks (PDFs) and answers natural language questions in English and Portuguese, with source attribution by page number.

- Qdrant gRPC client: **<1ms** search vs 60ms HTTP baseline
- 13.7x faster ingestion via batch embedding API
- bge-m3 multilingual embeddings for EN/PT cross-lingual retrieval
- OpenAI-compatible REST API — connects to Open WebUI without changes
- 11 Architecture Decision Records documenting every trade-off

**Stack:** Java 21 · Spring Boot 3.3 · Spring AI · Qdrant · PostgreSQL · Ollama · Docker

---

## Some things I've shipped

- Cut API response times **70%** on an ecommerce platform handling 1M+ daily requests (Estée Lauder)
- Reduced critical incident detection from **60 min → under 10 min** across 400+ microservices (Avangrid)
- Migrated 10M+ government records into Elasticsearch — data access from **2 min → 10 seconds** (TCU)
- Led PingOne OAuth2/OIDC integration end to end, delivered in 3 months (Yubico)
- Reduced auth defect rates **30%** for a platform serving 60K employees across 180 countries (CNH)

---

## Core stack

**Backend**
Java 21 · Spring Boot 3 · Spring Cloud · Spring Security · Quarkus · Kotlin

**Frontend**
React · TypeScript · Angular · Micro Frontends

**Data & Messaging**
Apache Kafka · Apache Spark · PostgreSQL · Elasticsearch · Redis · Oracle

**Infrastructure**
AWS (EKS · Lambda · S3 · CloudWatch) · Kubernetes · Docker · Terraform

**Observability**
Grafana · Prometheus · Kibana · OpenTelemetry

**Auth & Security**
OAuth2 · OIDC · JWT · OWASP · PingOne · Microsoft Entra ID

---

## Writing

I write about Java 21, distributed systems, and AI engineering on [LinkedIn](https://linkedin.com/in/sammyjdev).

Latest: [Building RAG in Java 21 — 9 Architecture Trade-offs from a D&D Query System](https://www.linkedin.com/pulse/building-rag-java-21-9-architecture-trade-offs-dd-query-sammy-junior)

---

📍 João Pessoa, Brazil · Open to US remote · English C2

[![LinkedIn](https://img.shields.io/badge/LinkedIn-sammyjdev-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/sammyjdev)
[![Email](https://img.shields.io/badge/Email-samjrdev98@gmail.com-D14836?style=flat&logo=gmail)](mailto:samjrdev98@gmail.com)
