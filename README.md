# Hekimcan Aktaş

**Software engineer · systems where correctness, privacy and auditability are requirements.**
.NET · PostgreSQL · Oracle PL/SQL · distributed data · İzmir, Türkiye

I design systems the way a reviewer would attack them. I start with the invariant that must never break, then
the failure that would break it, then the test that proves it holds. Most of my work sits where money, health
data and regulation meet. In those places "it works on my machine" is not evidence. A measured number and a
written trade-off are.

[hekimaktas.com](https://hekimaktas.com) · hekimcan206@gmail.com

---

## How I work

| Principle | What it looks like in my repositories |
|---|---|
| **Invariants first** | The database enforces them: deferred constraint triggers, append-only history, least-privilege roles. Property-based tests (FsCheck) attack them. |
| **Measure, don't claim** | Every performance number comes from a reproducible run. Each run records the hardware, the host load and its medians, in `docs/benchmarks.md`. |
| **Decisions are written down** | ADRs list the options I rejected and why. The README says what the system does *not* do. |
| **Real dependencies in tests** | Integration tests run against real PostgreSQL 17 and Oracle 23ai through Testcontainers, not in-memory fakes. |
| **Privacy by construction** | Synthetic data only. PII is classified, masked or encrypted by default. Each design answers who can see the data and whether access leaves a trace. |
| **One command to verify** | `docker compose up` runs the full demo, and CI runs that same demo and asserts the result. |

---

## Selected work

### [ledger-recon](https://github.com/hekimcan206/ledger-recon): double-entry ledger with exactly-once posting
PostgreSQL · .NET 10 · Npgsql · FsCheck · NBomber
- **50 identical concurrent requests → exactly 1 journal entry**, proven under both row-lock and `SERIALIZABLE` strategies.
- End-of-day reconciliation against card, EFT/FAST and MT940 files. It reports exactly the defects the generator injected, checked over 200 generated days per run.
- **1,185 RPS at p99 75.9 ms** with row locks, against 523 RPS under `SERIALIZABLE` with retryable aborts. The ADR explains why that difference decides the default strategy.

### [medula-precheck](https://github.com/hekimcan206/medula-precheck): healthcare billing rules as versioned code
.NET 10 · Oracle 23ai · PL/SQL · ODP.NET
- 22 effective-dated SUT rules across 3 rule versions. Each rule has golden pass and fail cases, a reason in Turkish and English, and a machine-readable fix.
- A set-based PL/SQL bulk path and the .NET engine produce **byte-identical findings on 3,000 invoices**.
- 142,305 invoices/s in .NET and 8,695 invoices/s end to end through Oracle. The benchmark also documents an execution-plan reuse regression it found.

### [kvkk-data-inventory](https://github.com/hekimcan206/kvkk-data-inventory): PII discovery and masking across Oracle and PostgreSQL
.NET 10 · Oracle · PostgreSQL · KVKK/GDPR
- 22 detectors combine column-name heuristics, value sampling and checksums. They score **precision/recall/F1 0.939** on a labelled schema with deliberate decoys, with identical results on both engines.
- The ablation shows why the signals are combined: names alone score F1 0.835 and values alone 0.79.
- Keyed one-way HMAC masking keeps TCKN checksums valid, gives the same pseudonym on both engines, and resists dictionary attacks without the key.

### [dotnet-service-template](https://github.com/hekimcan206/dotnet-service-template): the baseline every service starts from
`dotnet new hekim-service`: central package management, warnings as errors, OpenTelemetry, Serilog JSON, ProblemDetails, a chiseled non-root image (198 MB), Testcontainers and CI/CodeQL/release-please. 93.7% line coverage of the API code.

<details>
<summary><b>In progress</b>: same standard, landing one by one</summary>

| Area | Repositories |
|---|---|
| Healthcare & privacy | `patient-feedback-workflow` · `hbys-fhir-bridge` · `sut-rules` · `kvkk-dsar` · `field-crypto` · `merkle-audit` |
| AI infrastructure | `guarded-db-mcp` (secure MCP server for Oracle/PostgreSQL) · `rag-tr` · `tr-rag-eval` · `oracle23ai-rag` |
| Distributed systems | `offline-pos-sync` · `webhook-relay` · `idempotent-api` · `order-saga-temporal` · `cdc-pipeline` · `outbox-lab` |
| Databases & migration | `ora2pg-verify` · `saas-rls-starter` · `plan-guard` |
| Finance | `fx-bitemporal` · `bank-statement-recon` · `trkarekod` · `ubltr-validator` |
| Reliability | `slo-chaos-lab` · `flags-rollout` · `signalr-scale-lab` |

</details>

---

## Toolbox

**Languages:** C#, SQL, PL/SQL, Python, TypeScript
**Backend:** .NET 10 / ASP.NET Core, EF Core, Dapper, SignalR, Hangfire, OpenTelemetry
**Data:** PostgreSQL, Oracle (PL/SQL, 23ai), Redis, PostGIS, SQL Server; data modelling, query optimisation, CDC
**Infra:** Docker, GitHub Actions, OCI
**Testing:** xUnit v3, Testcontainers, FsCheck, NBomber, BenchmarkDotNet, Playwright

**Certifications:** OCI Certified Architect Associate (2026) · OCI Foundations Associate (2026) · Oracle AI Database Foundations Associate

## Open-source contributions

Contributions to upstream projects will be listed here once they are merged.

---

<sub>Every repository uses synthetic data only. No employer or customer code, schemas or data appear on this profile.</sub>
