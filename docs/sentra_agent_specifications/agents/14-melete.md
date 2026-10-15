# Melete — Research Strategy and Hypothesis Design

**Role ID:** AG-14  
**Module:** Intelligence and research  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Develop competing explanations and testable hypotheses, then identify minimal safe experiments that distinguish them and expose information gaps.

## 2. Intelligence and model strategy

Optional LLM/statistical reasoning for candidate generation; deterministic checks for testability, scope, evidence, and permitted operations.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Objective, baseline, asset graph, history, owner priorities, approved tools, policy constraints.

## 4. Outputs and contract

HypothesisSet with source evidence, counter-evidence, falsification criteria, discriminating tests, alternatives, questions.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only evidence search, graph queries, test templates, schema validator; no execution or approval.

## 6. Execution workflow

Retrieve evidence; separate observations from assumptions; propose alternatives; define supporting/falsifying evidence; map tests to approved templates; identify unsafe/non-discriminating tests; hand to Genesis/NEXARCH.

## 7. Governance boundaries

Cannot call hypothesis a finding, invent support, recommend out-of-scope actions, or initiate tools. Model confidence is not probability unless calibrated.

## 8. Evidence and audit

Hypothesis version, evidence IDs, rationale, alternatives, test IDs, model/template version.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Insufficient evidence -> state uncertainty; contradictory evidence -> preserve alternatives.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Each hypothesis has falsification criteria.
2. Citations resolve to evidence or are marked missing.
3. Out-of-scope actions are rejected.
4. Confidence is not mislabeled probability.
5. Manual/rule-based hypothesis input works without model.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
