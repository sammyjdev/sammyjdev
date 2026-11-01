# Sammy Junior

Senior AI Engineer. I ship AI that holds up in production.

I spent six years on backend systems in Java and Python before moving into AI, and I build LLM features with monitoring, tests and a fallback for when a dependency fails. In healthcare, I'm an AI engineer on the generative AI clinical assistant in a leading Brazilian clinical decision app, where I designed an LLM-as-judge evaluator for clinical answer quality, validated against physician review. At Avangrid, a regulated US utility, I took a RAG agent from proof of concept to a deployable architecture. An assumed similarity threshold of 0.85 made it refuse relevant questions, so I profiled the real scores and set it to 0.50, then chose the model by testing six candidates on 13 queries. On the backend side, I cut API response times 70% on an Estée Lauder platform serving more than a million requests a day, and brought incident detection at Avangrid under 10 minutes across 400+ microservices.

Any action with an outbound effect waits for human approval, and a post-validation step discards output the source does not support.

The repos below are tools I build and measure on my own. Each one publishes its numbers, including the ones that went against me.

## Projects

### [AXON](https://github.com/sammyjdev/axon)
Memory for AI coding agents. AXON stores the decisions, lessons and session history that the code does not show, and serves them to Claude Code, Codex and Cursor over MCP or a `.axon/context.md` fallback.

- Context is captured from git hooks and session hooks. There is no LLM in the capture path and no idle cost (dec-104).
- PostgreSQL full-text search is the only store. SQLite, Redis, Qdrant, mem0 and pgvector were evaluated and retired (dec-121); Neo4j was rejected (dec-101).
- Every MCP tool is classified read, write or destructive and runs behind a policy gate, with traces under a shared `trace_id` (ADR-013, dec-109).
- Usage showed agents ignored the code search, so I retired it. Full-text search then ranked the right lesson first on 41 of 41 trigger queries, against 33 for vector search, at about 5 ms per search instead of 600, and freed 578 MB of a 610 MB database (dec-138).
- 13 ADRs, 37 decision records, 1,529 tests passing in CI.

Stack: Python 3.11, MCP, PostgreSQL, Pydantic v2, pytest

### [GNOMON](https://github.com/sammyjdev/gnomon-eval)
LLM eval harness that reports every judge metric with a confidence interval. `pip install gnomon-eval`

- The case is the replication unit. Pooling repeated judge runs on the same case as independent data shrinks the interval by √N, so GNOMON bootstraps over cases, with intervals bounded to [0,1].
- The gate compares the lower bound of the interval, not the mean, and runs as a regression test (exit 0 or 1).
- Three judges from different vendor families. Verdicts are reported with disagreement stats and never averaged.
- Offline by default through Ollama, with no paid keys; hosted judges through OpenAI-compatible endpoints.
- On AXON's retrieval cases (N=34), context precision was about 0.83 with retrieval and 0.0 without.
- 532 tests, 12 ADRs.

Stack: Python 3.11, Pydantic v2, Ollama, pytest

### [GLYPH](https://github.com/sammyjdev/glyph-kg)
Knowledge-graph RAG for documents and code, measured against a vector baseline on the same corpus. `pip install glyph-kg`

- Documents go through LLM extraction and code through tree-sitter (Python and Java), into one graph core. NetworkX is the default store; a Neo4j adapter is optional.
- The graph thesis did not hold on code. On the AXON code graph (n=14), the vector baseline beat the graph on faithfulness under two independent judges (0.995 vs 0.839 with Gemini, 0.926 vs 0.862 with Qwen). On documents (n=25), faithfulness tied between 0.98 and 1.00 across arms.
- In a pre-registered benchmark against graphify (95k stars), whole-file vectors lost. With symbol-level retrieval, GLYPH reached statistical parity at about a third of the generator tokens. On code, the retrieval unit mattered more than the strategy.
- On documents, cross-encoder reranking raised context precision from 0.40 to 0.72 (95% CIs overlap).
- On sense-making questions over code (n=8), community summaries matched the other arms at about half the tokens (5.3k vs 10k+).
- 410 tests, 97% coverage, 8 ADRs.

Stack: Python 3.11, Pydantic v2, NetworkX, Neo4j, tree-sitter, sentence-transformers, pytest

### [MERIT](https://github.com/sammyjdev/merit) (archived)
A LangGraph agent that reads a job posting and says whether my profile fits.

- Six-node StateGraph with SQLite checkpoints, so a session that dies mid-run resumes where it stopped.
- The graph stops for human approval before any outbound effect.
- A deterministic check drops any verdict whose evidence is not literally in the profile.
- 95.4% mean agreement on a 114-verdict golden set from 29 real postings, against a CI gate of 80%.

Stack: Python, LangGraph, LangChain, LangSmith, SQLite

Also public: [RPG Master AI](https://github.com/sammyjdev/rpg-master-ai), a multilingual RAG system in Java 21 and Spring AI with 14 ADRs, and [gate over model](https://github.com/sammyjdev/gate-over-model), benchmarks showing that in an agentic TDD loop the verification gate limits quality more than the implementer model does.

## Earlier work
- Consolidated 20+ SQL sources into one Elasticsearch index of 10M+ records; data access went from 2 minutes to 10 seconds (TCU).
- Led a PingOne OAuth2/OIDC integration from design to delivery in 3 months (Yubico).
- Cut the critical defect rate 30% in access control for 60,000+ employees in 180 countries (CNH).

## Stack
Java, Python, TypeScript, Kotlin, Spring Boot, Spring AI, FastAPI, LangChain, LangGraph, LangSmith, MCP, PostgreSQL, pgvector, Elasticsearch, Redis, Kafka, AWS, GCP, Docker, Kubernetes, OpenTelemetry

Brazil. Remote (UTC-3), English C2.
[LinkedIn](https://linkedin.com/in/sammyjdev) · [Email](mailto:samjrdev98@gmail.com)
