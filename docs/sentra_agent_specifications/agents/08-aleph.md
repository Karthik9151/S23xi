# Aleph — Asset, Service, and Relationship Intelligence

**Role ID:** AG-08  
**Module:** Intelligence and research  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Build a provenance-aware map of registered lab assets, services, applications, identities, dependencies, network segments, and defensive controls.

## 2. Intelligence and model strategy

Deterministic parsers and graph correlation; optional AI summarizes or proposes candidate links only.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Owner lab inventory, authorized discovery outputs, configuration exports, service fingerprints, boundaries, evidence IDs.

## 4. Outputs and contract

Asset graph, relationship candidates, confidence/source references, conflicts, unknowns, coverage report.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only inventory, parsers for approved outputs, graph queries; no active discovery invocation.

## 6. Execution workflow

Load inventory/scope; parse supplied or Runner-produced artifacts; normalize identities; attach source and reality labels; flag stale/conflicting records; return only relevant context.

## 7. Governance boundaries

Cannot add targets to scope or initiate scans. Discovery outside scope is quarantined as observation and never tested automatically.

## 8. Evidence and audit

Source artifact, parser/tool version, observation time, identity rationale, confidence, reality label, conflicts.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Conflicting identity/stale inventory -> UNKNOWN/REVIEW_REQUIRED; parser errors remain errors, not guessed facts.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Out-of-scope asset is never promoted to scope.
2. Every fact has provenance or inferred/unknown label.
3. Conflicts remain distinct until reviewed.
4. Same artifact/parser version is deterministic.
5. Graph does not grant execution permission.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
