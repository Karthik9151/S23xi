# Kratos — Deterministic Policy Enforcement

**Role ID:** AG-05  
**Module:** Governance and authorization  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Evaluate proposed actions against versioned rules for exact scope, target identity, tool/action allowlists, tier, risk, limits, exclusions, and stop conditions.

## 2. Intelligence and model strategy

Deterministic code only for authoritative ALLOW/DENY/HOLD; AI may explain reason codes but cannot decide.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Canonical action request, mission/scope digest, target, tool version, approval reference, lab attestation, policy version, limits.

## 4. Outputs and contract

Integrity-protected PolicyDecision with ALLOW/DENY/HOLD, reason codes, matched rules, versions, validity window.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Policy engine, scope matcher, trusted tool registry, environment attestation, audit sink.

## 6. Execution workflow

Validate schema; resolve exact target; check allowlist/exclusions; enforce mode/risk/rate/time; check versions; emit decision; repeat at local execution boundary.

## 7. Governance boundaries

Cannot grant owner approval, rewrite policy from agent suggestions, allow unknown targets, or fail open. ERGOUSIARCH is doctrine; Kratos is the deterministic enforcement capability.

## 8. Evidence and audit

Action digest, policy version, target/scope, reasons, decision time, Gate instance, dependency health.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Missing policy, stale scope, ambiguous target, dependency outage, or evaluation error -> HOLD/DENY.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Out-of-scope/excluded targets are denied.
2. Stale mission versions are rejected.
3. Policy timeout fails closed.
4. Argument changes force re-evaluation.
5. AI explanation cannot change decision.
6. Both Gates are required for execution.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
