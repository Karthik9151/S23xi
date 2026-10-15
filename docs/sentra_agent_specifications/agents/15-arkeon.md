# Arkeon — Lab Lifecycle and Environment Stewardship

**Role ID:** AG-15  
**Module:** Execution and environment  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Manage registered lab lifecycle: identity, health, snapshots, reset/teardown, artifact staging, isolation validation, and cleanup verification.

## 2. Intelligence and model strategy

Deterministic lifecycle controller; no model required.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Lab profile, environment attestation, authorized lifecycle request, mission lease, quotas, active process inventory, cleanup policy.

## 4. Outputs and contract

LabStatus, preflight attestation, snapshot/restore/teardown results, isolation report, cleanup verification, environment digest.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Local hypervisor/container/lab APIs, firewall/egress checks, process supervisor, artifact staging; allowlisted admin operations only.

## 6. Execution workflow

Verify owner-controlled lab identity; check isolation/egress/resources; validate snapshot; attest to local Gate; monitor health; stop/clean on cancellation; verify residual resources.

## 7. Governance boundaries

Cannot expand routes, connect to arbitrary external targets, disable kill switch, or claim cleanup without proof. Isolation failure stops mission.

## 8. Evidence and audit

Lab profile/version, environment digest, isolation checks, snapshot IDs, lifecycle events, resource inventory, cleanup proof.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Isolation failure -> BLOCKED; cleanup uncertainty -> CLEANUP_PENDING and owner alert; lost heartbeat invokes local stop policy.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Unexpected egress route blocks execution.
2. Lab identity mismatch blocks mission.
3. Kill switch stops new actions and active processes as specified.
4. Cleanup requires verification.
5. Environment drift invalidates attestation.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
