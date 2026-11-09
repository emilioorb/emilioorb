# Emilio Rodríguez

**Software & AI Engineer** · Founder of [Arclo Systems](https://arclosystems.com) · Costa Rica

I design the architecture of Arclo's systems, lead a team of 4 developers, and I still write the code. I work in TypeScript end to end, across React Native, React/Next.js, NestJS and PostgreSQL, and I ship LLM features to production with evals and cost controls. I use Claude Code every day.

---

## AI engineering

- **Evals as a merge gate (Kodi):** a versioned golden dataset and an LLM-as-judge calibrated against human labels (Cohen's kappa), with bootstrap confidence intervals. CI blocks the merge when quality drops.
- **LLM gateway (Kodi):** model routing, failover to a second provider behind a circuit breaker, prompt caching, the Batch API, and a cost ledger with budgets and alerts. Changes ship through canary, shadow or A/B rollouts with a kill switch.
- **AI tutor in production (Kodi, 3,000+ users):** Claude explains each mistake and diagnoses patterns. Prompts are versioned per country, guardrails live in code (minor protection, prompt-injection defense), and output is validated with Zod. It has atomic daily quotas and logs the cost of every call.
- **Grounded RAG (Rige):** hybrid search (pgvector + Spanish full-text) with a reranker, plus a graph of laws that tracks amendments so answers cite the version in force. Every citation is verified character by character against the source, and the system answers "I don't know" without evidence. Evals measure Recall@k, MRR, citation accuracy and PII leakage.
- **MCP servers:** read access to ERP data for agents. On the invoicing API, an agent can issue invoices only within a mandate signed by a person.

## Production systems

### [Kodi](https://holakodi.com): exam-prep app (Android & iOS), 3,000+ users

Adaptive practice across 5 modules, real-time 1v1 duels, leagues and streaks.

- NestJS + Prisma + PostgreSQL + Redis. Isolated BullMQ queues (gamification, push, Play Store order accounting) run idempotent jobs.
- Leaderboards on Redis sorted sets, Redis rate limiting, PostgreSQL advisory locks, S3 presigned URLs.
- In-app billing through Google Play RTDN (Pub/Sub) and App Store Server Notifications. JWT with refresh-token rotation, Argon2, social login and passkeys.
- Expo + EAS mobile app with iOS widgets in Swift. Next.js admin panel (~150 pages), Astro landing page.

### [Transportes Acaya](https://acaya.app): logistics platform, 1,500+ trips

Replaced the client's spreadsheet. It is now the system they invoice from.

- 4-state trip lifecycle, per-client rate engine, purchase orders, 3-role RBAC, real-time notifications.
- Hacienda v4.4 e-invoicing with XAdES signatures made with the taxpayer's `.p12`.
- Playwright + axe (WCAG 2.2 AA) E2E tests, plus LCP/CLS and bundle-size gates in CI.

### Arclo ERP: internal, 11 modules

CRM, projects, finance with CRC/USD double-entry accounting, procurement, and a client portal.

- Mandatory 2FA and passkeys. Deny-by-default permissions are checked against the database on every request. The audit log is hash-chained with SHA-256 and anchored daily to object storage.
- Testcontainers integration tests, 84% API unit coverage, an 80% gate on changed lines, and 16 migrations, each with a tested rollback. CI also runs gitleaks, osv-scanner, Opengrep and dependency-cruiser.

## Projects

### [T-Ledger](https://github.com/emilioorb/T-Ledger) · [live](https://t-ledger.vercel.app) (open source)

Double-entry personal accounting for individual, couple and family ledgers.

- Optimistic concurrency: versioned writes across 12 entities return `409` on stale edits. ADR-006 picks a per-ledger lock after measuring that serializable transactions failed 27 of 120 concurrent writes.
- Found a cross-ledger data leak in a raw SQL query. I reproduced it with a failing test, fixed it, and added a guard test that rejects any raw query without a ledger id.
- 89% API line coverage, 6 ADRs, a pre-push verification gate, WCAG 2.2 AA.

### [Electronic Invoicing API](https://emiliorb.com/proyectos/api-facturacion) (private repo)

Costa Rica's Hacienda v4.4 with no third-party SDKs.

- XAdES-EPES signing on `node:crypto`, verified by a separate library. XSD validation runs in an isolated process. The `.p12` and PIN live only in memory.
- Multi-company issuing, a retry queue, webhooks, a legal archive, and OpenAPI with a client library. 8 ADRs.

### [Rige](https://emiliorb.com/proyectos/gaceta-ai) (private repo)

RAG over Costa Rica's official gazette since 2015. The PDF signature is verified on ingestion and personal data is stripped before storage. NestJS, pgvector, pg-boss, Claude.

## Architecture

- **Style:** modular monoliths with DDD-style domain modules and hexagonal boundaries. Slow or failure-prone work moves to isolated queues.
- **Reliability:** idempotent jobs and idempotency keys, retries with circuit breakers, cache-aside, optimistic concurrency.
- **APIs:** REST with OpenAPI, RFC 9457 errors, cursor pagination, webhooks, API keys.
- **Decisions on paper:** ADRs (8 on the invoicing API, 6 on T-Ledger), and SRS documents (ISO/IEC/IEEE 29148) with use cases and traceability.
- **Standards for humans and agents:** Arclo's engineering standards are served over MCP, and Claude Code reads them before writing code. I work spec-driven.

## Stack

| | |
|---|---|
| Languages | TypeScript, JavaScript, SQL, PL/SQL, Swift |
| Frontend | React, Next.js, Astro, Vite, Angular, TanStack, Tailwind CSS, shadcn/ui |
| Mobile | React Native, Expo (EAS), Ionic |
| Backend | NestJS, Node.js, Express, Prisma, WebSockets, BullMQ, pg-boss |
| Data | PostgreSQL, pgvector, Redis, SQL Server, Oracle |
| AI | Claude API, RAG, reranking, MCP, structured output, LLM evals |
| Testing | Jest, Vitest, Playwright, Testcontainers, axe, Cypress, Selenium |
| Infra | AWS, Firebase, Vercel, Railway, Cloudflare, GitHub Actions, Sentry |

## Before Arclo

**Novelteak** · 2024–2025: co-built an offline-first Ionic + Angular app for field crews with no signal, and the multi-department platform behind it (Express + SQL Server). I built its RBAC and super-admin panel and wrote stored procedures in SQL Server and Oracle PL/SQL.

**GSIT** · 2022–2024: led concurrent Scrum projects for banking, retail, tourism and industrial clients in 3 countries. I defined the QA standard the company adopted, and on the 8 projects I tested there were zero critical defects in production.

## Elsewhere

[emiliorb.com](https://emiliorb.com) · [LinkedIn](https://www.linkedin.com/in/emiliojrb/) · [X](https://x.com/_emiliojrb) · [emiliorb@arclosystems.com](mailto:emiliorb@arclosystems.com)
