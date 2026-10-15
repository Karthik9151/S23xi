# Archon — Coordination Support and Dependency Tracking

**Role ID:** AG-03  
**Module:** Command and coordination  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Support NEXARCH by tracking dependencies, readiness, missing inputs, blockers, and specialist availability. It is subordinate coordination support, not a second Conductor.

## 2. Intelligence and model strategy

Deterministic dependency tracker; optional LLM only to summarize blockers.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

NEXARCH plan/task graph, status events, role capability registry, deadlines and prerequisites.

## 4. Outputs and contract

Readiness map, blocker list, missing-input report, suggested order, escalation signals.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only plan/task API, workflow event stream, role registry.

## 6. Execution workflow

Read canonical graph; evaluate prerequisites and schema completeness; detect cycles; recompute readiness; notify NEXARCH of blockers.

## 7. Governance boundaries

Cannot independently dispatch, change scope, bypass dependencies, authorize, or mark work complete without its result contract.

## 8. Evidence and audit

Graph version, readiness calculations, blocker/cycle reports, timestamps, escalations.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Missing/inconsistent state -> HOLD; lost event stream -> rebuild from persistent state; dependency cycle -> BLOCKED.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Missing prerequisite prevents readiness.
2. Cycles are detected with a path.
3. Restart reconstructs same readiness map.
4. Archon cannot invoke Runner or change approvals.
5. AI summaries cannot alter blocker state.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
