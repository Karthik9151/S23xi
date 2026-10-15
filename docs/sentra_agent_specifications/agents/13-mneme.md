# Mneme — Evidence Memory, Retrieval, and Provenance

**Role ID:** AG-13  
**Module:** Intelligence and research  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Index and retrieve mission history, artifacts, findings, baselines, and decisions with provenance so claims link to canonical evidence.

## 2. Intelligence and model strategy

Deterministic metadata retrieval; optional semantic search only resolves to canonical records.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Mission/task/evidence IDs, owner query, access scope, provenance graph, retention rules, approved index.

## 4. Outputs and contract

Ranked evidence references, source metadata, provenance chain, freshness, missing-record notice, duplicates.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only evidence API, controlled artifact reader, search index, hash verifier; no evidence mutation.

## 6. Execution workflow

Authenticate caller; filter by workspace access; search metadata/optional semantic index; resolve canonical records; verify hashes; return source IDs/labels; audit access.

## 7. Governance boundaries

Cannot fabricate artifacts, treat semantic similarity as proof, leak other workspaces, or alter raw evidence.

## 8. Evidence and audit

Query reference, index version, canonical IDs, access decision, hash result, retrieval timestamp.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Index outage -> exact lookup fallback if safe; hash mismatch -> quarantine and alert; absent item -> NOT_FOUND.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Every result resolves to canonical evidence.
2. Search respects workspace access.
3. Hash mismatch prevents intact presentation.
4. Missing evidence is reported as missing.
5. Labels/provenance survive indexing/export.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
