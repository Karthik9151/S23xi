# Origo — Independent Verification and Finding Adjudication

**Role ID:** AG-12  
**Module:** Independent verification  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Determine independently whether a proposed finding is supported and reproducible. Use a different method when feasible and report limitations.

## 2. Intelligence and model strategy

Deterministic assertions/independent checkers first; optional model may classify evidence but cannot certify.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Finding candidate, evidence manifest, baseline, verification policy, alternative methods, authorized scope, verification artifacts.

## 4. Outputs and contract

VerificationRecord: VERIFIED, NOT_REPRODUCED, DISPUTED, INCONCLUSIVE, or BLOCKED; method, evidence links, limitations.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only evidence store, assertions, approved verifier wrappers invoked through Gate/Local Runner; cannot edit evidence or approve runs.

## 6. Execution workflow

Validate provenance; define falsifiable criteria; choose independent method; request separately gated run if needed; compare results; preserve discrepancies; issue verdict.

## 7. Governance boundaries

Cannot approve its own run, rely on narrative alone, edit raw evidence, or mark missing evidence verified.

## 8. Evidence and audit

Claim digest, evidence/baseline IDs, method/version, Gate/approval references, outcome, verdict.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Unsafe/unavailable method -> BLOCKED/INCONCLUSIVE; conflict -> DISPUTED; failed reproduction is not proof of safety.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Claim without evidence cannot be VERIFIED.
2. Independent method is used or limitation documented.
3. Conflicts yield DISPUTED/INCONCLUSIVE.
4. Verifier failure never becomes VERIFIED.
5. Raw evidence remains unchanged.
6. Reports separate verified and unverified findings.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
