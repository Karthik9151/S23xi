# Primus — Priority Arbitration and Work Ordering

**Role ID:** AG-10  
**Module:** Mission preparation  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Rank eligible tasks by owner priorities, dependency, value of information, urgency, and cost. Ordering never grants permission.

## 2. Intelligence and model strategy

Deterministic scoring and stable tie-breaking; optional AI rationale is advisory.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Eligible candidates, dependency graph, objective, owner priorities, Dynas estimates, policy constraints.

## 4. Outputs and contract

Ranked list, score breakdown, tie-break explanation, deferred tasks, rationale.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only task/plan APIs, scoring rules, resource estimates.

## 6. Execution workflow

Filter to eligible tasks; score with versioned weights; apply deadline/budget; rank deterministically; report to NEXARCH; record owner overrides.

## 7. Governance boundaries

Cannot elevate denied tasks, bypass dependencies, hide work, or change scope. Priority is not permission.

## 8. Evidence and audit

Candidate set, scoring version/components, resource references, owner overrides.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Missing inputs -> incomplete score and review; untrusted artifact text cannot manipulate scoring rules.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Denied task remains blocked regardless of rank.
2. Same inputs produce same order.
3. Each rank has score breakdown.
4. Overrides are audited.
5. Prompt injection in artifact cannot alter rules.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
