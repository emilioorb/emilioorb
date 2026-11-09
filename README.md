# Emilio Rodríguez

**Software & AI Engineer** · Founder of [Arclo Systems](https://arclosystems.com) · Costa Rica

Software and AI engineer, TypeScript end to end: mobile (React Native + Expo), web (React, Next.js, Astro) and backends (NestJS, PostgreSQL, Redis). I lead a team of 4 developers and take products from design to production. I work daily with Claude Code and ship LLM features into products: an AI tutor, RAG, MCP servers and evals.

---

## Production systems

### [Kodi](https://holakodi.com): exam-prep app (Android & iOS), 3,000+ users

Adaptive practice across 5 modules (COSEVI driver's license, UCR/UNA/TEC admission, national exams and more), plus real-time 1v1 duels, leagues, streaks and missions.

- **Backend:** NestJS + Prisma + PostgreSQL + Redis, organized as domain modules. Separate BullMQ queues (gamification, FCM push, Google Play order accounting) with idempotent jobs. Leaderboard cache on Redis `ZSET`, Redis rate limiting, PostgreSQL advisory locks, S3 presigned URLs.
- **Billing:** Google Play Billing (RTDN via Pub/Sub), App Store Server Notifications, AdMob with SSV.
- **Auth:** JWT with refresh-token rotation, Argon2, Google/Apple/Facebook sign-in, passkeys.
- **AI tutor (Claude Haiku 4.5):** explains each mistake with follow-ups, writes session and weekly summaries, runs a proactive diagnosis after 3 misses in a row and generates questions. Prompts are versioned per country and editable from the admin panel. Guardrails live in code (minor protection, prompt-injection defense, Socratic method). Atomic daily quotas, token and cost logging per call.
- **LLM evaluation and control:** versioned golden dataset, LLM-as-judge calibrated with Cohen's kappa, bootstrap confidence intervals, and a CI gate that blocks merges when quality drops. Cost ledger with budgets and alerts, Batch API, prompt caching, model routing, a gateway with failover to a second provider and a circuit breaker, Zod-validated structured output, canary/shadow/A-B rollout with a kill switch, and a human review queue.
- **Mobile:** Expo SDK 57, React Native 0.86, Expo Router, EAS Build/Update/Submit, Zustand + TanStack Query, Reanimated, Skia, iOS widgets in Swift, Firebase.
- **Web:** admin panel in Next.js 16 + React 19 (~150 pages, shadcn/ui, Tailwind 4), landing page in Astro on Vercel.
- **Testing:** Jest (unit + e2e), Vitest + Playwright, GitHub Actions. Sentry, Pino, OpenAPI.

`React Native` `Expo` `NestJS` `PostgreSQL` `Redis` `BullMQ` `Socket.IO` `Claude API` `Next.js` `AWS S3`

### [Transportes Acaya](https://acaya.app): logistics platform, 1,500+ trips

Replaced the client's spreadsheet. It is now the system they invoice from.

- 4-state trip lifecycle, per-client rate engine, purchase orders, procurement module, RBAC with 3 roles, passkeys, real-time notifications, Excel/PDF reports.
- Electronic invoicing: Hacienda v4.4 XML generation and XAdES signing with the taxpayer's `.p12`.
- Quality: Playwright + axe (WCAG 2.2 AA) E2E tests, LCP/CLS gates, bundle budget, GitHub Actions.

`React 19` `Vite` `NestJS` `PostgreSQL` `Playwright`

### Arclo ERP: internal, 11 modules

CRM, projects, finance, contracts, documents, tickets, tasks, procurement and a read-only client portal.

- Double-entry accounting in CRC/USD with BCCR exchange rates.
- **Security:** mandatory 2FA and passkeys (Better Auth), deny-by-default permissions checked against the DB on every request, a SHA-256 hash-chained audit log (Postgres trigger, daily anchor to an R2 bucket with retention), least-privilege DB roles, strict CSP + HSTS.
- **Quality:** Testcontainers integration tests, Playwright + axe E2E, 84% API unit coverage, an 80% gate on changed lines, 16 migrations each with a tested rollback, plus gitleaks, osv-scanner, Opengrep and dependency-cruiser in CI.
- MCP server that exposes ERP data to AI agents.

`React 19` `Vite PWA` `TanStack Router` `NestJS` `Prisma` `PostgreSQL` `Zod` `Railway` `Vercel`

---

## Projects

### [Electronic Invoicing API](https://emiliorb.com/proyectos/api-facturacion)

In-house API for Costa Rica's Hacienda v4.4 with no third-party SDKs.

- XAdES-EPES signing with `node:crypto`, 50-char key + sequence number, XSD validation in an isolated process, and a separate library that independently verifies every signature.
- Multi-company issuing, supplier-invoice intake, retry queue, PDF/webhook/email delivery, legal archive, API keys, OpenAPI and a client library. The `.p12` and PIN live only in memory.
- AI: CABYS classification, rejection explanations, expense classification, a draft D-150 VAT return with flagged inconsistencies. MCP server where an agent can issue invoices only within a mandate signed by a person.
- 8 ADRs.

### [Rige](https://emiliorb.com/proyectos/gaceta-ai) (private repo)

RAG over Costa Rica's official gazette, with history back to 2015.

- Daily ingestion with PDF digital-signature verification and private-person PII removed before storage.
- Hybrid search (pgvector + Spanish full-text), a reranker, and a graph of laws that follows amendments and repeals so answers cite the version in force.
- Every citation is verified character by character against the official page. It answers "I don't know" when there is no evidence, and retrieved text is treated as untrusted.
- Fixed plan → retrieve → generate → verify pipeline: Claude Sonnet generates, Claude Haiku classifies. Token, cost and latency logging with a daily spend cap.
- Evals: Recall@k, MRR, literal-citation rate, PII leakage, faithfulness judged by a hand-validated judge.

`NestJS` `PostgreSQL` `pgvector` `pg-boss` `Claude` `MCP` `Next.js`

### [T-Ledger](https://github.com/emilioorb/T-Ledger) · [live](https://t-ledger.vercel.app)

Double-entry personal accounting for individual, couple and family ledgers: trial balance, bank reconciliation, multi-currency net worth.

- 89% API line coverage, 6 ADRs, and a verification gate that runs before every push.
- Optimistic concurrency: versioned writes across 12 entities, with `409` on stale edits. Per-ledger lock (ADR-006), chosen after measuring that serializable transactions failed 27 of 120 concurrent writes.
- I found a cross-ledger leak in a raw SQL query, reproduced it with a failing test, fixed it, and added a guard test that rejects any raw query without a ledger id.
- Passkeys, axe WCAG 2.2 AA, LCP 2.44 s on slow 4G.

`NestJS` `Prisma` `PostgreSQL` `React 19` `TanStack` `Tailwind 4`

---

## Stack

| Area | Production |
|---|---|
| Languages | TypeScript, JavaScript, SQL, PL/SQL, Python, Swift, C#, Bash |
| Frontend | React, Next.js, Astro, Vite, Angular, TanStack Query/Router, Tailwind CSS, shadcn/ui, PWA |
| Mobile | React Native, Expo (EAS), Ionic, offline-first, in-app payments, push |
| Backend | NestJS, Node.js, Express, Prisma, REST/OpenAPI, GraphQL, WebSockets, BullMQ, FastAPI |
| Data | PostgreSQL, pgvector, Redis, SQL Server, Oracle, MySQL, Firestore |
| AI | Claude API, AWS Bedrock, OpenAI API, RAG, hybrid search, reranking, MCP, tool calling, structured output, LLM evals, LLM-as-judge, guardrails, Langfuse |
| Security | Passkeys (WebAuthn), 2FA, OAuth/OIDC, JWT, Argon2, RBAC/ABAC/ReBAC, XAdES/PKI, OWASP Top 10 |
| Testing | Jest, Vitest, Playwright, Testcontainers, Cypress, Selenium, axe, fast-check, load testing |
| Cloud & DevOps | AWS, Google Cloud, Firebase, Vercel, Railway, Cloudflare, Docker, GitHub Actions, Terraform, Sentry |

**How I build:** domain modules (DDD-style), hexagonal architecture, idempotent jobs, cache-aside, retries plus circuit breakers, RFC 9457 errors, cursor pagination, ADRs, SRS documents per ISO/IEC/IEEE 29148, and blameless postmortems. Spec-driven development with Claude Code. Arclo's engineering standards are published on an MCP server that agents read before they write code.

## Before Arclo

**Novelteak** · 2024–2025: co-built an offline-first Ionic + Angular app for field crews with no signal, and the multi-department platform behind it (Express + SQL Server). Built the RBAC and super-admin panel, wrote stored procedures and triggers in SQL Server, PostgreSQL and Oracle PL/SQL, and resolved production incidents.

**GSIT** · 2022–2024: led concurrent Scrum projects for banking, retail, tourism and industrial clients in 3 countries. Defined the QA standard the company adopted, and built automated suites (Cypress, Selenium, Jest, Postman) with zero critical defects in production across 8 projects.

## Elsewhere

[emiliorb.com](https://emiliorb.com) · [LinkedIn](https://www.linkedin.com/in/emiliojrb/) · [X](https://x.com/_emiliojrb) · [emiliorb@arclosystems.com](mailto:emiliorb@arclosystems.com)
