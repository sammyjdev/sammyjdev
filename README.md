# Hey, I'm Sammy 👋
I build production LLM systems, RAG pipelines, AI agents, MCP integrations, and context engines, on a full-stack foundation in Java 21 / Spring Boot and React/TypeScript. Senior AI Engineer with six years of distributed systems before my first model call, remote with US companies, based in Brazil.

What separates my work: I treat AI systems like real engineering. Every project carries ADRs documenting the trade-offs, releases gate on TDD, and when I wanted to know if my RAG system was actually good, I built an eval harness that caught the standard LLM-as-judge math inflating confidence by √N. My context engine AXON measurably cuts input tokens 85.5% at p50 in production telemetry; I retired an earlier 52.3% claim once I found out it was a deterministic model, not a measurement.

Currently building open-source AI infrastructure under **SJ AI Solutions** (AXON, GLYPH, GNOMON, shipped on PyPI and GitHub). Previously at **Avangrid**, where I drove incident detection from 60 minutes to under 10 across 400+ microservices.

---

## What I'm building

### Open source

### 🧠 [AXON](https://github.com/sammyjdev/axon)
Agent-agnostic context engine that gives AI coding agents continuous memory across sessions, tools, and machines.

Switching between Claude Code, Codex, or Cursor forces every agent to start from scratch. AXON captures context at solidification points — git commits and session boundaries — and surfaces it back via MCP or a `.axon/context.md` fallback, so any agent inherits prior decisions without a re-briefing. Self-hosted, event-driven, running in production with real sessions.

- 85.5% p50 / 78.8% mean input-token reduction across 69 real context windows (reproducible: `python -m axon.observability.compression_telemetry`); bracketed by a deterministic 20-round benchmark at 52.3%. Separately, 90.7% real-usage savings vs reading each source file in full (32,298 vs 346,081 tokens, 100 requests)
- Unified storage: PostgreSQL as source of truth with pgvector for vector retrieval; graph-aware retrieval delegated to GLYPH (dec-116). Qdrant, Redis, mem0, and SQLite all evaluated and retired, consolidated onto Postgres (dec-121); Neo4j evaluated and rejected (dec-101)
- Strictly event-driven capture via session hooks and git actions (post-commit, push, init) — zero idle cost, no background polling (dec-104)
- Task-based routing across free and paid tiers: paid uses Claude Haiku 4 / Sonnet 4 / Opus 4 (OpenRouter); free falls back to Llama 3.1-8B and 3.3-70B (Groq) and Llama 3.1-70B (NVIDIA NIM); Ollama for offline paths; classifier on gpt-oss-120b
- 34 ADRs + 119 decision records with explicit supersession chains; TDD-gated chunker as release barrier; Pydantic v2 domain models throughout; 1,536 tests

**Stack:** Python 3.11 · MCP (stdio) · PostgreSQL · pgvector · Pydantic v2 · Ollama · Claude API · pytest

---

### ⚔️ [RPG Master AI](https://github.com/sammyjdev/rpg-master-ai)
Production-grade RAG system that ingests RPG rulebooks (PDFs) and answers natural language questions in English and Portuguese, with source attribution by page number.

- **9.2× faster ingestion** by batching embeddings (80 chunks → 5 Ollama calls, bge-m3 local) — reproducible benchmark in-repo
- Qdrant accessed over **gRPC** (architecture choice; on localhost gRPC ≈ REST at ~2.5ms, edge grows under load/concurrency)
- bge-m3 multilingual embeddings for EN/PT cross-lingual retrieval
- Dual retrieval ports (Spring AI + LangChain4j) behind one interface; OpenAI-compatible REST API — connects to Open WebUI without changes
- 14 Architecture Decision Records documenting every trade-off; ArchUnit test enforces the hexagonal boundary (no Spring AI / Qdrant / PDFBox / JPA imports in the application layer)

**Stack:** Java 21 · Spring Boot 3.3 · Spring AI · LangChain4j · Qdrant · PostgreSQL · Ollama · Docker

Is it actually any good? I built **[GNOMON](https://github.com/sammyjdev/gnomon-eval)** to measure that honestly, and caught the standard LLM-as-judge confidence interval being off by √N.

---

### 📐 [GNOMON](https://github.com/sammyjdev/gnomon-eval)
Evaluation harness for my RAG systems (RPG Master AI, and AXON retrieval): scores RAG answers with an LLM judge and reports faithfulness + context precision with statistically honest confidence intervals. Offline-first, Apache-2.0.

- Caught the standard LLM-as-judge CI as pseudoreplication: running a deterministic judge N times and pooling `cases × runs` inflates `n` and shrinks the interval by √N — `[0.225, 0.775] n=16` (fake) vs `[0.0, 1.0] n=2` (honest). Fix: the case is the replication unit, percentile bootstrap over cases, bounded to [0,1] by construction
- Three-judge panel across distinct model vendor families with capacity screening; all verdicts reported with disagreement stats, never averaged
- Gate compares the CI lower bound, not the mean; exposed as a regression test (exit 0/1)
- Benchmarked AXON's retrieval context precision from 0.05 → 0.83 under a three-judge majority gate (95% bootstrap CIs, N=34)
- Offline Ollama judge, zero paid keys on the default path; hosted judges via OpenAI-compatible endpoints
- 12 ADRs; 352 tests; stdlib-only beyond Pydantic v2

**Stack:** Python 3.11 · Pydantic v2 · Ollama · Docker · pytest · ruff

---

### 🕸️ [GLYPH](https://github.com/sammyjdev/glyph-kg)
Knowledge-graph library for RAG spanning **document** (LLM extraction) and **code** (tree-sitter) domains behind one extractor port, benchmarked against a fair vector baseline.

Most "graph RAG" demos never prove the graph beats plain vector search. GLYPH builds the graph, builds an honest vector baseline on the same corpus, and measures both with GNOMON — graph wins faithfulness on documents, vector wins on code, and community summaries match quality at ~half the tokens. Reported with bootstrap confidence intervals, not vibes.

- Two extraction adapters behind one `Extractor` port: `DocumentExtractor` (LLM, probabilistic) and `CodeExtractor` (tree-sitter Python + Java, deterministic) — opposite edge-reliability models, one shared graph core
- `GraphStore` port with two real backends: NetworkX default (embedded, pip-installable, zero server) and a Neo4j adapter (openCypher) — both pass the same 14 contract tests, the Neo4j one smoke-tested against Neo4j 5 on Testcontainers
- Cross-encoder reranking improved context precision 80% over the vector baseline (0.72 vs 0.40, GNOMON-benchmarked, 95% CIs); graph-aware retrieval (entity anchoring + neighborhood expansion) + hybrid fusion scored across document (n=25), code (n=14), and global-community (n=8) axes
- 420 tests, ~97% coverage; 8 ADRs (dec-g1..g8); the `GraphContextSource` boundary is what AXON consumes for graph-aware retrieval (dec-116)

**Stack:** Python 3.11 · Pydantic v2 · NetworkX · Neo4j · tree-sitter · sentence-transformers · GNOMON · pytest

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
- Drove PingOne OAuth2/OIDC integration end to end, delivered in 3 months (Yubico)
- Reduced auth defect rates **30%** for a platform serving 60K employees across 180 countries (CNH)

---

## Core stack
**AI & ML**
LLMs · RAG · GraphRAG · AI Agents · Multi-Agent Systems · MCP · Spring AI · LangChain4j · Qdrant · pgvector · Ollama · Claude API · OpenAI API

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

📍 Brazil · Open to US remote · English C2

[![LinkedIn](https://img.shields.io/badge/LinkedIn-sammyjdev-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/sammyjdev)
[![Email](https://img.shields.io/badge/Email-samjrdev98@gmail.com-D14836?style=flat&logo=gmail)](mailto:samjrdev98@gmail.com)
