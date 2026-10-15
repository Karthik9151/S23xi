# Monarque — Intent Interpretation and Clarification

**Role ID:** AG-02  
**Module:** Command and coordination  
**Status:** Proposed design; not implemented or tested in a running system.

## 1. Purpose and responsibility

Convert the owner's natural-language goal into a structured mission draft with assumptions, target candidates, exclusions needing clarification, desired evidence, and measurable success criteria.

## 2. Intelligence and model strategy

Optional LLM for language interpretation; deterministic normalization/schema validation; manual intake fallback.

Model output is untrusted data. It must pass typed schema validation and relevant deterministic controls. Model text is never authorization, a policy decision, or evidence by itself.

## 3. Inputs

Owner request, explicitly referenced workspace context, approved templates, registered lab inventory, required intake fields.

## 4. Outputs and contract

IntentDraft: objective, rationale, proposed targets (not authorization), assumptions, open questions, requested evidence, success criteria.

Machine outputs should include schema version, mission/task IDs, status, timestamps where relevant, provenance references, and explicit errors/limitations. Free-form prose must not be the only machine interface.

## 5. Tools and permissions

Read-only templates/inventory, intake API, schema validator.

## 6. Execution workflow

Parse the goal; distinguish owner facts from assumptions; identify target/environment/action ambiguity; ask questions; emit typed draft; pass completeness state to NEXARCH.

## 7. Governance boundaries

Cannot infer permission, turn a public target into approved scope, create approvals, or convert draft directly into executable mission.

## 8. Evidence and audit

Request reference with sensitive data minimized, draft version, assumptions/questions, model/template version, validation errors.

Raw tool output should be retained as a read-only artifact with cryptographic hash, timestamp, tool/wrapper version, environment identity, and reality label. Derived interpretations link to raw evidence and remain separate. Corrections create new linked records.

## 9. Failure handling

Missing/ambiguous target -> NEEDS_CLARIFICATION; model failure -> manual intake or INCOMPLETE; never invent missing facts.

All failures need machine-readable status and reason codes. Unavailable policy, approval, scope, environment identity, or audit dependencies fail closed for consequential actions.

## 10. Acceptance tests

1. Ambiguous target remains non-executable.
2. Assumptions are explicitly labeled.
3. Model-added targets require owner confirmation and scope approval.
4. Malformed output fails validation.
5. Manual intake works without an AI provider.

## 11. Handoff

This role uses the shared task/result contract in `../modules/INTERFACES_AND_HANDOFFS.md`. It hands typed outputs to NEXARCH or the named downstream role and does not rely on hidden conversational memory.
