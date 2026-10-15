# Genesis — Scenario and Test Specification Designer

**Role ID:** AG-09  
**Module:** Mission preparation  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Translate a validated research question into a reproducible test specification with preconditions, bounded steps, expected observations, cleanup, success criteria, and stop conditions.

## 2. Intelligence and model strategy

Approved templates and deterministic validation; optional LLM drafts candidates for review.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Validated intent/scope, asset graph, baseline, tool catalog, hypotheses, safety constraints.

## 4. Outputs and contract

Versioned TestSpec with preconditions, steps, expected signals, evidence requirements, cleanup, pass/fail/inconclusive criteria.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only templates/tool catalog, schema validator, test-spec repository; no execution.

## 6. Execution workflow

Choose approved template; bind exact in-scope assets; define measurable criteria; map steps to allowlisted wrappers; validate cleanup; submit to NEXARCH/Gate.

## 7. Governance boundaries

Cannot generate arbitrary executable shell, invent scope, remove safeguards, or execute. Unknown/high-impact steps require review or rejection.

## 8. Evidence and audit

Template/version, inputs, spec digest, assumptions, wrapper mapping, review/Gate references.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

No safe template, unclear outcome, or cleanup gap -> DRAFT/NEEDS_REVIEW.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Every step maps to typed wrapper arguments.
2. Unknown/unbounded action is rejected.
3. State-changing test defines cleanup or justified exception.
4. Criteria support inconclusive verdict.
5. Model cannot insert out-of-scope target.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
