# Shared Interfaces and Handoff Rules

**Status:** Design contract only.

## Canonical task envelope
Every handoff includes schema version; mission ID/version; task ID and parent ID; requesting and assigned role; objective/operation; exact asset references and scope digest; input record IDs with provenance/reality labels; expected output schema; policy version and approval reference if needed; deadline/resource/time budgets; retry policy; stop conditions; cancellation reference; status/reason codes.

Persist and version the envelope. Conversational context alone is never sufficient for consequential work.

## Result envelope
Every result includes schema version, mission/task IDs, role and implementation version; status (`SUCCESS`, `PARTIAL`, `NEEDS_CLARIFICATION`, `BLOCKED`, `FAILED`, `CANCELLED`, `UNKNOWN`, `INCONCLUSIVE`); structured payload; evidence/provenance references; reality labels; assumptions/limitations; timestamps; resource usage/retries where relevant; error codes; integrity digest where appropriate. Task `SUCCESS` does not mean a security finding is verified.

## Reality labels
- `lab-observed`: directly produced by approved run in identified lab.
- `simulated`: produced by synthetic/simulated environment.
- `inferred`: interpretation, not direct observation.
- `user-supplied`: provided by owner, not independently validated.
- `external-reference`: imported documentation/intelligence.
- `unknown`: provenance/reality cannot be established.

Never silently convert one label into another.

## Authority and decision ordering
1. Owner request creates intent, not permission.
2. Monarque drafts intent; ambiguity stops progression.
3. NEXARCH plans; Archon checks dependencies.
4. Genesis defines test spec; Ab-Initio supplies required baseline.
5. Dynas bounds resources; Primus ranks only eligible tasks.
6. Kratos evaluates policy; Exousia validates owner-issued approval.
7. Control-plane Gate decision supports scheduling but does not suffice for local execution.
8. Arkeon validates lab isolation; local Gate rechecks scope, approval, exact action digest, expiry, quota, and stop conditions.
9. Protos requests Local Runner execution using registered wrappers only.
10. Raw evidence is hashed and committed.
11. Mneme indexes canonical evidence; Aleph correlates asset facts; Melete proposes evidence-linked hypotheses.
12. Origo independently verifies.
13. NEXARCH reports verified and unverified results separately.

## Retries and cancellation
Retry automatically only when an action is idempotent, with bounded count/backoff. Non-idempotent or state-changing work requires review before retry. Cancellation prevents new work immediately. A lost heartbeat is not proof that execution stopped. Any change to scope, action, tool version, target, rate, or limits invalidates the action digest and forces re-gating.

## Evidence manifest
Include artifact ID/hash, mission/task/run IDs, tool/wrapper version, timestamp/clock source, lab identity/attestation, scope/action digests, Gate/approval references, reality label, sensitivity/redaction class, storage and retention metadata. A hash proves consistency with a digest, not that a claim is true.

## Report eligibility
- `VERIFIED`: Origo's documented independent criteria pass.
- `NOT_REPRODUCED`: verification did not reproduce claim; not proof of safety.
- `DISPUTED`: credible evidence conflicts.
- `INCONCLUSIVE`: insufficient evidence or incomplete verifier.
- `BLOCKED`: prerequisites/policy disallowed verification.
Unverified candidates must be in a separate section.

## Security invariants
No agent has unrestricted shell access, grants approval, widens scope, or bypasses Gates. Only Local Runner invokes registered tools. Hosted control plane has no direct target route. Missing policy, approval, scope, environment, or audit dependencies fail closed. Secrets stay out of model prompts and ordinary logs.
