# NEXARCH — Sole Conductor and Mission Orchestrator

**Role ID:** AG-01  
**Module:** Command and coordination  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Own the persisted mission workflow, decompose the approved objective into bounded tasks, coordinate dependencies, collect results, and assemble an evidence-linked report.

## 2. Intelligence and model strategy

Deterministic workflow/state machine; optional LLM only for draft decomposition or summaries after schema validation.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Mission/version, validated scope, policy and approval references, task graph, baseline/evidence IDs, deadlines, budgets, cancellation state.

## 4. Outputs and contract

Versioned plan, typed task envelopes, workflow transitions, escalations, final report manifest and evidence links.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Mission API, workflow tables/queue, typed agent adapters, audit API, read-only evidence metadata.

## 6. Execution workflow

Load canonical mission; require intent completeness; draft dependency graph; request Gate and approval validation; check baseline/resources; dispatch eligible tasks; recheck versions/cancellation before consequential handoffs; collect evidence; request Origo; report verified and unverified results separately.

## 7. Governance boundaries

Cannot authorize, widen scope, self-approve, bypass Gates, directly access targets, or mark its own claims verified. Material plan changes require re-gating.

## 8. Evidence and audit

Plan digests, task assignments, transitions, Gate/approval references, retries, cancellations, tool versions, report-evidence links.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Ambiguity -> NEEDS_CLARIFICATION; denial -> BLOCKED; safe idempotent failures may retry within bounds; verifier unavailable -> UNVERIFIED; cancellation stops new work and reconciles active runs.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Out-of-scope tasks are never dispatched.
2. NEXARCH cannot mint or approve authorization.
3. Same persisted inputs and versions yield the same workflow decisions.
4. Expired approval blocks dispatch.
5. Cancellation prevents new tasks and records active-run disposition.
6. Origo failure never yields VERIFIED.
7. Plan changes update digest and force revalidation.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
