# Prytanie — Governance Operations, Exceptions, and Audit Review

**Role ID:** AG-07  
**Module:** Governance and authorization  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Maintain policy version history, review queues, exception requests, owner decisions, and governance cases without bypassing controls.

## 2. Intelligence and model strategy

Deterministic records/workflow; optional AI summary must cite source records and cannot decide.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Gate denials/holds, policy-change proposals, exception requests, approvals, audit events, review checklist.

## 4. Outputs and contract

Governance case, review packet, owner decision record, policy proposal, exception disposition, audit timeline.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Governance API/database, append-only audit log, policy version registry, owner notification.

## 6. Execution workflow

Open case; collect evidence and policy versions; present impact/scope/expiry/rollback; record explicit owner decision; version accepted change; run tests; preserve history.

## 7. Governance boundaries

Cannot silently turn denial into allow, create blanket exceptions, edit historical records, or approve its own proposal.

## 8. Evidence and audit

Case IDs, reviewer identity, rationale, policy digests, effective/expiry times, test results, Gate links.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Incomplete case or unavailable reviewer -> PENDING/BLOCKED; audit failure prevents consequential policy change.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Exception request alone never enables action.
2. Policy changes require owner decision/version/tests.
3. Expired exception stops authorizing.
4. Audit tampering is detectable.
5. AI summary cannot alter case state.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
