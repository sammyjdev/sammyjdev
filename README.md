# Hey, I'm Sammy 👋
Senior AI Engineer based in Brazil, working remotely with US companies for 3+ years.
I build production LLM systems — RAG pipelines, AI agents, MCP integrations, and context engines — on top of a backend foundation in Java 21 and Spring Boot.
Currently at **Avangrid** — payment processing and billing platforms for 400K+ energy customers.

---

## What I'm building

### Open source

### 🧠 [AXON](https://github.com/SamDev98/axon)
Agent-agnostic context engine that gives AI coding agents continuous memory across sessions, tools, and machines.

Switching between Claude Code, Codex, or Cursor forces every agent to start from scratch. AXON captures context at solidification points — git commits and session boundaries — and surfaces it back via MCP or a `.axon/context.md` fallback, so any agent inherits prior decisions without a re-briefing. Self-hosted, event-driven, running in production with real sessions.

- 52.6% reduction in p50 input tokens measured across real sessions (n=7) via selective context retrieval — reproduced by a deterministic 20-round benchmark at 52.3%
- Segregated storage: SQLite as source of truth, Redis as graph cache, Qdrant + mem0 for vector retrieval — Neo4j evaluated and rejected (dec-101)
- Strictly event-driven capture via session hooks and git actions (post-commit, push, init) — zero idle cost, no background polling (dec-104)
- Task-based cloud routing: Haiku 4.5 for trivial ops, Sonnet 4.6 for code analysis, Opus 4.7 for architectural reasoning — Ollama (phi3:mini, gemma4) for offline paths
- 6 ADRs (dec-100 → dec-105), TDD-gated chunker as release barrier, Pydantic v2 domain models throughout

**Stack:** Python 3.11 · MCP (stdio) · SQLite · Redis · Qdrant · mem0 · Pydantic v2 · Ollama · Claude API · pytest

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

### 📊 [Claude Usage Bar](https://github.com/SamDev98/claude-usage-bar)
Cross-platform menu bar app that surfaces Claude Pro/Max quota usage at a glance.

Single Rust binary that reads the locally stored Claude Code OAuth token, polls the usage endpoint every 5 minutes, and renders 5-hour and weekly rolling limits as a color-coded tray icon with a detailed menu. Shipped for macOS, Linux, and Windows via GitHub Actions.

- Portable core with compile-directive isolation for OS-specific concerns (token discovery, login autostart) — only two boundary modules
- Built on `tao` + `tray-icon`, `ureq` 2.x pinned for HTTP API stability — fully synchronous runtime, ~10 direct dependencies
- CI matrix covering macOS, Linux, and Windows with a release workflow producing signed `.app` packages and per-OS artifacts
- Tray icon generated dynamically from live usage state — green/orange/red thresholds, text label in macOS menu bar

**Stack:** Rust · tao · tray-icon · ureq · serde · GitHub Actions

---

### 🧠 [JobFlow AI](https://github.com/SamDev98/jobflow-ai)
Multi-tenant SaaS for job application management with AI-powered resume optimization and Kanban pipeline tracking.

- OpenAI-compatible AI integration via OpenRouter with Redis cache (TTL 7 days) — cuts token cost significantly
- Clerk JWT auth with per-tenant isolation via Spring Security
- Full CI/CD: Java backend on Fly.io + React frontend on Vercel
- Demo mode decoupled from backend — runs frontend-only with mock data for portfolio

**Stack:** Java 21 · Spring Boot 3.5 · React 18 · TypeScript · PostgreSQL · Redis · Docker · Clerk

---

### Private

### 🌊 Pharos *(private)*
Document validation pipeline for Brazilian customs brokers — pre-Siscomex compliance checks across multi-exporter workflows.

**Stack:** Python 3.11 · FastAPI · SQLAlchemy 2.0 async · Postgres + pgvector · Redis · S3 · Docker Compose

---

### 💅 Lumé *(private)*
Vertical SaaS for the Brazilian beauty market — scheduling, POS, commissions, and AI-driven WhatsApp automation.

**Stack:** Kotlin 1.9 · Spring Boot 3.3 · PostgreSQL · jOOQ · Next.js 14 · Claude API · WhatsApp Cloud API

---

### 🛰️ Orion-AI *(private)*
AI agent embedded in ClickUp that converts task webhooks and chat events into structured project management actions via Claude Haiku.

**Stack:** Python · FastAPI · Claude Haiku · PostgreSQL · ClickUp MCP · Docker

---

## Some things I've shipped
- Cut API response times **70%** on an ecommerce platform handling 1M+ daily requests (Estée Lauder)
- Reduced critical incident detection from **60 min → under 10 min** across 400+ microservices (Avangrid)
- Migrated 10M+ government records into Elasticsearch — data access from **2 min → 10 seconds** (TCU)
- Led PingOne OAuth2/OIDC integration end to end, delivered in 3 months (Yubico)
- Reduced auth defect rates **30%** for a platform serving 60K employees across 180 countries (CNH)

---

## Core stack
**AI & ML**
LLMs · RAG · AI Agents · MCP · Spring AI · LangChain4j · Qdrant · pgvector · Ollama · Claude API · OpenAI API

**Backend**
Java 21 · Spring Boot 3 · Spring Cloud · Spring Security · Python · FastAPI · Kotlin · Rust

**Data & Messaging**
Apache Kafka · PostgreSQL · Elasticsearch · Redis · Apache Spark · Oracle

**Infrastructure**
AWS (EKS · Lambda · S3 · CloudWatch) · Kubernetes · Docker · Terraform

**Observability**
Grafana · Prometheus · Kibana · OpenTelemetry

**Auth & Security**
OAuth2 · OIDC · JWT · OWASP · PingOne · Microsoft Entra ID

---

## Writing
I write about AI engineering, Java 21, and distributed systems on [LinkedIn](https://linkedin.com/in/sammyjdev).
Latest: [Building RAG in Java 21 — 9 Architecture Trade-offs from a D&D Query System](https://www.linkedin.com/pulse/building-rag-java-21-9-architecture-trade-offs-dd-query-sammy-junior)

---

📍 João Pessoa, Brazil · Open to US remote · English C2

[![LinkedIn](https://img.shields.io/badge/LinkedIn-sammyjdev-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/sammyjdev)
[![Email](https://img.shields.io/badge/Email-samjrdev98@gmail.com-D14836?style=flat&logo=gmail)](mailto:samjrdev98@gmail.com)
