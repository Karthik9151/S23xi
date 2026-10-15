# Technology Stack and Hosting Decisions

**Status:** proposed stack based on the owner's preferences. No deployment is configured or authorized by this document.

## Chosen starting direction

| Layer | Direction | Rationale / boundary |
|---|---|---|
| API/control plane | Python + FastAPI + Pydantic | Typed request/response boundaries and a small modular API |
| Database | PostgreSQL | Durable missions, owner/session data, scope/approval versions, jobs, audit and evidence metadata |
| ORM/migrations | SQLAlchemy 2 + Alembic | Parameterized persistence and reviewed schema evolution |
| Frontend | React + TypeScript + Vite | Single-owner mission, scope review, progress, evidence, verification and report UI |
| CLI | Python; Typer is a candidate | Main operator path for local mission/scope operations and Runner preflight |
| Local Runner | Python service/CLI on owner's machine | Local Gate, fixed wrappers, process control, limits and evidence capture |
| Tests | pytest; PostgreSQL integration; frontend build and browser tests | Verify both component behavior and security boundaries |
| Mission state | PostgreSQL-backed explicit state machine and leases initially | Avoid extra workflow infrastructure until failure/recovery needs demonstrate value |
| Raw evidence | Local filesystem, content hash and manifest; permissions/append-only conventions | Keep sensitive raw output local by default; database contains metadata and references |
| Graph/3D/event platform | Deferred | Not required for the current product direction; add only if measurable need justifies complexity |
| Model provider/framework | Undecided, not required | The first version must be useful and testable without a model |

## Frontend and backend security compatibility

If the React app and API are hosted, prefer a reviewed same-origin arrangement or another explicitly tested topology. Do not assume cookie/CSRF behavior from local development will work on a hosted deployment. Test cookie domain, `Secure`, `SameSite`, CORS allowlist, CSRF token, origin checks, login/logout and refresh behavior in the actual staging topology.

Single-user design is not a reason to remove auth. Public registration remains disabled. Use an owner bootstrap procedure and revoke/disable bootstrap after the owner is provisioned. Require MFA (for example, TOTP) before exposing the control plane to the internet. If password login is used, choose a modern password hash such as Argon2id. Browser sessions are server-managed, opaque, expiring and revocable. CLI credentials are separate and revocable; never store long-lived secrets in the frontend bundle.

## Database requirements

- PostgreSQL is the supported persistent database for integration acceptance.
- SQLAlchemy queries must be parameterized/ORM-driven; no SQL formed by concatenating untrusted input.
- Alembic migrations must be reviewed and tested against PostgreSQL, not just SQLite.
- SQLite may be used as a fast unit-test convenience only where it does not mask PostgreSQL behavior. PostgreSQL integration and concurrency tests remain required for acceptance.
- Do not store raw secrets, complete authorization headers or unredacted sensitive payloads in metadata tables.

## Temporary staging on Render

Render is a possible temporary host for the **control plane and UI only**, after an owner-approved deployment decision. Before using it, recheck current plan pricing, free-tier limits, card/billing requirements, service sleep behavior, database costs, logs/retention, region and data handling. Do not assume a free tier is permanent or necessarily card-free.

Hard boundary: the Runner and lab stay local. Render must not have VPN/route credentials or direct reachability to a target. Raw evidence remains local by default; any upload requires a separate privacy/redaction and data-transfer decision. If the required no-charge conditions cannot be confirmed, do not deploy.

## Later AWS or Cloudflare option

AWS and Cloudflare are possible future directions, not a combined stack choice. Evaluate a concrete architecture only after S9/S11 entry conditions: where the API runs, how the React app is served, database ownership/backups, identity and secrets, private networking, cost, audit/logging, region, and exit/migration plan. For Cloudflare, distinguish static UI/edge functionality from the Python FastAPI API and PostgreSQL hosting; do not assume one product provides every required component. Keep the Runner local regardless of provider.

## Dependencies deliberately not chosen yet

No Temporal, Kafka/Redpanda, graph database, multi-agent framework, 3D rendering engine, or paid LLM API is part of the starting requirements. The initial design uses typed modules, PostgreSQL state, a CLI, fixed wrappers, and basic UI. Add a dependency only through a decision record that names the requirement, alternative, cost/security impact, and acceptance test.

## Budget rule

The budget remains $0 through S4. After S4, paid infrastructure is still prohibited unless the owner creates and approves a decision record with current pricing, billing safeguards, data-handling/security analysis, expected usage, rollback/exit path, and a reason local/free options do not suffice.
