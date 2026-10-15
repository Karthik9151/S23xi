# Ab-Initio — Baseline Establishment and Snapshot Integrity

**Role ID:** AG-11  
**Module:** Mission preparation  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Capture the pre-test state needed to interpret results: inventory, configuration, service behavior, defensive state, timestamps, tool versions, and coverage gaps.

## 2. Intelligence and model strategy

Deterministic collectors/parsers and manifest generation; optional AI may summarize differences with citations.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Authorized mission/assets, approved read-only collection profile, lab identity, tool catalog, completeness requirements.

## 4. Outputs and contract

Baseline manifest, hashes, provenance, coverage, gaps, reality labels, immutable baseline ID.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Approved read-only Runner wrappers, hashing, metadata store; collection only after Gate checks.

## 6. Execution workflow

Verify scope/read-only profile; request Gate and Runner preflight; collect artifacts; hash raw files; validate coverage; freeze manifest; return baseline ID.

## 7. Governance boundaries

Cannot run mutating probes in read-only mode, invent missing observations, overwrite prior baseline, or expand scope.

## 8. Evidence and audit

Baseline ID, artifact hashes, timestamps, wrapper versions, targets, omissions, environment attestation, manifest digest.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Partial collection -> PARTIAL with gaps; hash or lab identity mismatch invalidates dependent baseline.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Identical content yields identical hash.
2. Partial coverage is not complete.
3. Raw baseline cannot be overwritten normally.
4. Stale scope/attestation blocks collection.
5. Every field is sourced, inferred, or unknown.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
