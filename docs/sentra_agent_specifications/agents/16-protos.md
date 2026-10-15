# Protos — Bounded Execution and Tool-Run Coordination

**Role ID:** AG-16  
**Module:** Execution and environment  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Coordinate an approved wrapper invocation through Local Runner, collect raw output, enforce per-run bounds, and return execution metadata. Not a general-purpose shell agent.

## 2. Intelligence and model strategy

Deterministic wrapper registry/process supervisor; model may suggest wrapper ID but cannot execute directly.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Task envelope, Gate decision, approval validation, scope/action digest, wrapper ID/version, quota, lab attestation, stop conditions.

## 4. Outputs and contract

RunRecord with status, wrapper/version, redacted args, exit code, limits, artifact IDs/hashes, output references, failure reason.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Local Runner IPC/API, allowlisted wrappers for port/service scan, HTTP/TLS, passive web baseline, defensive logs/detection, timeout/cancellation, artifact writer.

## 6. Execution workflow

Validate envelope/version; confirm local Gate, approval, lab attestation, allowlisted wrapper and quota; execute exact typed args; enforce limits; hash output; persist read-only artifact; return RunRecord; stop on revocation/drift/kill.

## 7. Governance boundaries

Cannot run free-form shell, mutate approved args, execute from hosted control plane, disable audit, or label findings verified. Lab-only scope is mandatory.

## 8. Evidence and audit

Run ID, mission/action/scope digests, Gate/approval IDs, lab attestation, wrapper/tool versions, times, limits, exit status, hashes, stop/cleanup state.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Timeout kills process group and preserves partial output; stale Gate/approval blocks; crash -> UNKNOWN until reconciled.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Unknown wrapper/arbitrary shell is rejected.
2. Changed target/rate invalidates digest.
3. Timeout kills process tree and preserves partial evidence.
4. Missing/stale Gate blocks run.
5. Raw output is hashed and cannot be silently overwritten.
6. Kill switch stops new runs and records disposition.
7. Hosted control plane has no direct target execution route.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
