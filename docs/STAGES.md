# Sentra Stage Roadmap (Fresh Sequence)

**Status:** proposed, awaiting owner review. These are new Sentra stages; they are not a rename of NEXORION Stage 1–5.  
**Precondition:** design-only under Charter C-1. No stage implementation starts until the owner explicitly approves implementation.

## Stage overview

| Stage | Name | Main outcome | Can real tools run? | Exit gate |
|---|---|---|---|---|
| S0 | Design and contract freeze | Approved architecture, mission/scope schemas, risk controls, stack decisions, test map | No | Owner accepts the documents and open decisions needed for S1 |
| S1 | Single-owner application foundation | FastAPI, PostgreSQL/SQLAlchemy migrations, owner bootstrap, sessions/CSRF, audit base, React shell | No | Auth/session/CSRF and owner isolation tests pass; database migrations work on PostgreSQL |
| S2 | CLI and mission governance | CLI, versioned mission/scope files, approval binding, Gate 1, mission state machine | No | Malformed, expired, out-of-scope, or unapproved actions are denied and recorded |
| S3 | Local Runner and Gate 2 | Local-only runner, lab preflight, typed wrapper interface, kill switch, no cloud-to-target route | No real scanning until exit acceptance | Isolation, target restriction, cancellation, and failure-path tests pass in a disposable lab |
| S4 | First real-lab vertical slice | Port/service discovery plus evidence manifest, independent verification, and report | Yes, after S3 acceptance; own isolated lab only | Gate denial tests, Nmap wrapper tests, hash verification, Origo rejection test, kill switch test, report tests pass. Spending remains $0 through this stage |
| S5 | HTTP and TLS baseline | Scoped HTTP probe and TLS configuration check | Yes, only after per-mission approval | Redirect/host-boundary tests, timeouts, limits, certificate evidence, and verification tests pass |
| S6 | Web baseline assessment | Conservative OWASP ZAP baseline against an approved training target; no unrestricted active attack profile | Yes, only against lab target and approved action list | Scan limits, allowed host handling, evidence labels, report limitations, and stop conditions pass |
| S7 | Defensive detection pillar | Import lab logs, map observed events to detection rules, test expected alerts and gaps | Log/detection work in lab only | Reproducible test events, traceable log evidence, true/false-positive tests, and useful detection report pass |
| S8 | Evidence and workflow hardening | Recovery, idempotency, redaction, artifact retention, backups, dependency/security checks, full failure tests | Existing wrappers only | Recovery, cancellation, DB consistency, audit linkage, and restore tests pass |
| S9 | Hosting decision and staging | Optional temporary Render control-plane staging; same-origin/session/CSRF behavior checked | Runner remains local; hosting never gets a target path | Owner approves deployment; current free-tier/billing terms and data boundaries checked; hosted smoke test and rollback evidence recorded |
| S10 | Optional model-assisted planning review | Decide whether to add an LLM; compare against a no-model baseline | No new tool authority from a model | Separate owner decision; model cannot bypass schema/Gates; privacy, cost, and adversarial tests pass before adoption |
| S11 | Future platform hardening | Evaluate AWS or Cloudflare, backup/restore, secrets, monitoring, operations, and exit plan | No T4 implied | A separate decision record chooses a target platform and verifies operational readiness |

S9–S11 are not automatic commitments. A stage may be deferred or cancelled if cost, risk, or evidence does not justify it. No paid resource is allowed without a written decision record.

## Stage-by-stage requirements

### S0 — Design and contract freeze

Deliverables: Charter; architecture/data-flow diagrams; mission/scope/approval schemas; Gate behavior; Local Runner contract; agent registry and module ownership; lab topology; threat model; evidence manifest; tool allowlist; reporting model; test matrix; security decisions; and unresolved owner decisions. Reconcile all cross-document conflicts. Do not write application code.

**Exit:** owner reviews the package and confirms the design baseline. This is not the C-1 implementation approval unless the owner explicitly says so.

### S1 — Single-owner application foundation

Proposed build: FastAPI/Pydantic API; SQLAlchemy models and Alembic migrations; PostgreSQL as the supported persistent database; one owner/workspace; explicit owner bootstrap; Argon2id password hashing if password login is selected; server-managed opaque sessions; HTTP-only cookies; CSRF protection on state-changing browser requests; session expiry, rotation and revocation; structured errors; request IDs; audit events; and a minimal React/TypeScript shell. Do not expose registration publicly. CLI and browser auth must not share a long-lived unscoped credential.

Tests: login/logout, invalid login, session expiry/revocation, missing/invalid CSRF token, cookie attributes, unauthorized owner/resource access, migration upgrade/downgrade or documented irreversible migration policy, and PostgreSQL integration.

### S2 — CLI and mission governance

Implement mission drafts, scope validation, approval records, action allowlists, policy versioning, a state machine, deterministic Gate 1, and an audit trail. A scope file identifies lab assets (prefer explicit addresses/ports over broad ranges), exclusions, permitted wrapper IDs and arguments, resource/time limits, rate/concurrency limits, expiry, and stop conditions. No target-touching wrapper is available yet; use fake wrappers and test fixtures.

Tests: allow, deny, malformed scope, out-of-scope address, public address, expired approval, changed scope hash, unknown wrapper, excessive limits, replayed approval, and contradictory policy all fail safely.

### S3 — Local Runner and Gate 2

The Runner stays on the user's machine. Gate 2 independently reads a locally controlled scope and policy configuration; the hosted control plane cannot mutate it. It checks approved mission identity, scope digest, target resolution, lab-network interface/routing, tool version, wrapper schema, time budget, and kill-switch state immediately before each wrapper call. Wrapper arguments are structured and allowlisted; no arbitrary shell commands, `shell=True`, user-supplied executable paths, or pass-through arbitrary tool flags. A network/isolation preflight failure blocks the run.

Tests run only in a disposable, authorized lab fixture: egress boundaries, redirect/DNS scope escapes, wrapper argument injection, runner disconnect, timeout, process-group termination, duplicate job, expiry mid-run, and STOP/kill-switch behavior.

### S4 — First real-lab vertical slice

Add one narrow port/service inventory wrapper (Nmap is the proposed candidate), artifact capture, SHA-256 manifest, tool/version/config metadata, finding normalization, a distinct verification method where appropriate, and a Markdown/JSON report. This is the first stage where a real tool may execute—and only after S3 acceptance and explicit approval for the specific lab mission.

Proposed verification examples: compare the discovered port/service with a separate local service check or lab-side listener inventory; Origo must describe the method and limitations. Re-running the same scanner with the same settings is not automatically independent verification.

Exit tests must show an allowed lab target succeeds, an out-of-scope target is blocked at both Gates, a false/unsupported finding is rejected, evidence tampering is detected, cancellation is logged, and reports never promote unverified findings to verified status.

### S5 — HTTP and TLS baseline

Use an allowlisted HTTP probe (for example, a constrained `curl` wrapper or equivalent typed client) and a TLS configuration checker (for example, `testssl.sh`) against explicitly scoped lab URLs. Restrict redirects and DNS resolution to the authorized scope; cap requests, concurrency, response bytes, duration, and redirect count; redact secrets and sensitive headers; record certificate and negotiated-protocol evidence. Keep checks conservative and non-destructive.

### S6 — Web baseline assessment

Start with a conservative OWASP ZAP baseline/passive assessment against a designated lab training application. Explicitly distinguish passive baseline checks from active scanning. No unrestricted spidering, credential attacks, exploit automation, destructive checks, or denial-of-service profiles in the initial scope. Active checks require a later explicit lab-only design and approval, not an implicit tool-option change.

### S7 — Defensive detection pillar

Import known lab logs or collect from explicitly designated lab systems using read-only mechanisms. Build repeatable test events, map them to detection rules, and evaluate whether expected telemetry and alerts exist. Sigma-compatible rules may be evaluated through a reviewed local pipeline. A missing log is evidence of an observation gap, not proof that an event did not occur. Reports include event time ranges, source integrity, collection gaps, rule version, and false-positive/false-negative limitations.

### S8 — Hardening and recovery

Test timeouts, retries, cancellation, runner crashes, stale leases, evidence partial writes, PostgreSQL failure, restore from backup, retention/deletion, redaction, dependency audit, secrets scanning, and audit linkage. Only retry actions that are explicitly safe and idempotent. Do not restart an interrupted scan automatically without a fresh scope/approval check.

### S9 — Hosting decision and staging

Render may be considered only for the control plane/UI. Before deploying, review current plan limits, billing/card requirements, database pricing, region/data handling, secret configuration, TLS, cookie domain and SameSite behavior, CSRF origins, owner bootstrap, and public exposure. Raw evidence and target logs remain local by default. The Runner does not move to Render. If free/no-charge operation cannot be confidently established, skip hosted staging and record the blocker.

### S10 — Model decision

The owner decides whether an LLM adds enough value to justify privacy, operational, reliability, and cost risk. Compare model-assisted intake/planning against deterministic templates and a no-model baseline. If added, the model may draft scope and hypotheses but cannot widen scope, create approvals, call arbitrary tools, alter policy, or certify findings. Keep provider integration mocked in tests and do not call a billable service without written approval.

### S11 — Future hosting and operations

Compare AWS and Cloudflare against concrete needs and budget. Define deployment topology, key/secret management, backup/restore, monitoring, data residency, incident response, vendor exit, and recovery objectives. This stage neither authorizes T4 nor permits hosted target access.

## Universal exit-gate checklist

A stage cannot be marked complete based on code presence alone. Record: acceptance commit/revision; commands/tests actually run; results; environment; limitations; known issues; security review; owner sign-off; and whether the next stage is authorized. Any unresolved critical scope/authorization/isolation finding blocks progression.
