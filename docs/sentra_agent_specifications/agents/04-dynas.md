# Dynas — Resource, Quota, and Capacity Planning

**Role ID:** AG-04  
**Module:** Mission preparation  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Estimate and bound runtime, concurrency, request rate, CPU/memory, artifact volume, storage, and retries.

## 2. Intelligence and model strategy

Deterministic quota engine; model estimates are advisory and bounded by hard caps.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Task plan, tool profiles, lab capacity, owner limits, safety caps, historical measurements.

## 4. Outputs and contract

ResourceEstimate, admissible envelope, quota reservation, schedule constraints, capacity warning/rejection.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only metrics/tool profiles, quota service, reservation API.

## 6. Execution workflow

Estimate demand; apply hard caps; reserve capacity atomically; provide envelope; monitor consumption; request stop on limits.

## 7. Governance boundaries

Cannot raise hard caps, grant permission, remove rate limits, or oversubscribe budgets.

## 8. Evidence and audit

Input profile, estimate method, requested/approved quotas, actual usage, threshold events, reservation lifecycle.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Unknown costs -> conservative bound or BLOCKED; quota service failure -> fail closed for consequential runs.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Above-cap rate is rejected.
2. Concurrent reservations cannot oversubscribe.
3. Quota exhaustion requests stop and is recorded.
4. Unknown estimate never becomes unlimited.
5. Reservations reconcile after cancellation.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
