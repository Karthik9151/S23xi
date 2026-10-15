# Cross-Agent Acceptance Matrix and Release Gates

**Status:** Proposed test plan; tests have not been run.

## Minimum for every agent
- Typed input/output schema tests, including invalid/missing/unexpected fields.
- Mission/workspace authorization tests.
- Provenance and reality-label preservation.
- Failure/timeout and audit behavior.
- Model-output validation if model-assisted.
- Restart/replay tests for persisted workflows.
- Cancellation tests when the role participates in active work.
- Negative tests proving permissions are bounded.

## Cross-agent scenarios
| ID | Scenario | Expected result |
|---|---|---|
| X-01 | Ambiguous target request | Monarque asks; mission remains non-executable |
| X-02 | NEXARCH proposes out-of-scope task | Gate denies; no Runner call |
| X-03 | Approval expires after planning | Local Gate blocks |
| X-04 | Model adds target/tool argument | Schema/scope/action digest rejects |
| X-05 | Discovery finds out-of-scope asset | Aleph records observation only; no test |
| X-06 | Quota exhausted | Protos stops/refuses and records reason |
| X-07 | Lab isolation fails | Arkeon/local Gate block |
| X-08 | Runner times out with partial output | Process stopped; partial evidence preserved |
| X-09 | Artifact hash mismatch | Quarantine and alert |
| X-10 | Origo cannot reproduce candidate | NOT_REPRODUCED/INCONCLUSIVE, never VERIFIED |
| X-11 | Origo unavailable | No verified status for pending candidates |
| X-12 | Cancellation during run | No new tasks; reconcile stop/cleanup |
| X-13 | Hosted service attempts direct target connection | Architecture test proves no execution route |
| X-14 | Mneme semantic retrieval | Every hit resolves to authorized canonical evidence |
| X-15 | Policy changes after planning | Revalidation before execution |
| X-16 | Agent restarts mid-mission | Restore from persistent state without unsafe duplicate action |
| X-17 | Target artifact contains prompt injection | Treat as untrusted data; no authority changes |
| X-18 | Audit storage unavailable | Consequential execution/change fails closed |
| X-19 | Result lacks required evidence | Incomplete; not promoted to finding |
| X-20 | High-priority task is denied | Remains blocked; priority is not permission |

## Real-tool release gate
Before any real lab tool runs:
1. Scope and exclusions are machine-checkable.
2. Owner authentication and action-bound approval pass tests.
3. Both Gates independently check exact action digest.
4. Runner accepts registered wrappers and typed arguments only.
5. Lab identity, isolation, egress restrictions, resource caps, and kill switch are tested.
6. Cancellation, timeout, crash recovery, and cleanup are tested.
7. Evidence is hashed and protected against silent overwrite.
8. Audit failure blocks consequential execution.
9. An out-of-scope target is demonstrably never contacted.
10. Origo can return negative, disputed, inconclusive, and blocked verdicts.

Acceptance requires test commands, environment/version, results, relevant evidence, known limitations, and reviewer decision. A checklist alone is not proof.
