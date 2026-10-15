# Exousia — Approval and Authorization Validation

**Role ID:** AG-06  
**Module:** Governance and authorization  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Validate that a human-issued approval is authentic, current, unrevoked, and bound to the exact mission, target scope, action, tool, limits, and validity period.

## 2. Intelligence and model strategy

Deterministic identity/signature/store validation; no LLM decision.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Approval reference, authenticated owner identity, action/scope digests, requested operation, current time, revocation status.

## 4. Outputs and contract

ApprovalValidation: VALID, INVALID, EXPIRED, REVOKED, MISMATCH, or UNAVAILABLE with reason codes.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Authentication/session service, approval store, signature verifier, revocation list, trusted clock, audit sink.

## 6. Execution workflow

Authenticate principal; load immutable approval; verify integrity/authority; compare scope/action; check expiry/revocation; return validation; recheck before every consequential run.

## 7. Governance boundaries

Cannot approve, extend, broaden, or reuse approval for a different action. Login session is not mission approval; agent is never approver.

## 8. Evidence and audit

Approval digest, principal, bound mission/action/scope, times, validation result, revocation check.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Identity, clock, signature, or store uncertainty -> unavailable/invalid and block execution.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Approval for target A cannot authorize target B.
2. Different action/tool fails match.
3. Expired/revoked/tampered approvals fail.
4. Agent cannot extend or mint approval.
5. Revocation is effective at next boundary check.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
