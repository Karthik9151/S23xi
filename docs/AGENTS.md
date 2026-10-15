# Agent and Module Registry

**Design rule:** preserve the 16 named roles, but do not build sixteen separate autonomous LLM agents by default. Use typed Python modules/services and deterministic code first. Add model-backed roles only after a separate owner decision and measurable benefit.

## Organizational boundaries

- **Sentra** is the product/workspace and durable mission/evidence record.
- **NEXARCH** is the **only primary Conductor**.
- **ERGOUSIARCH** is the governance doctrine enforced by deterministic code; it is not an unconstrained agent.
- **Origo** is the independent verifier and cannot verify its own output.
- **Archon** provides bounded coordination support under NEXARCH; it is not a competing orchestrator.
- A named role is a responsibility boundary, not permission to act. Only the Gates can authorize a wrapper call.

## Seven modules / sixteen named roles

### Module 1 — Mission orchestration

**NEXARCH — Sole Conductor**

Plans and coordinates the mission, tracks state, assigns typed tasks, aggregates outcomes, and returns the report. It cannot approve itself, change the owner-approved scope, bypass a Gate, or mark its own claim verified.

**Archon — Coordination support**

Tracks dependencies, delegated task status, deadlines, and handoff completeness under NEXARCH. It cannot create a competing plan authority or direct Runner execution outside a task already allowed by the Conductor and Gates.

### Module 2 — Intent and research

**Monarque — Intent interpreter**

Converts the user's natural-language request into objective, assumptions, missing details, and questions. It must ask for clarification instead of guessing target ownership or authorization.

**Melete — Research strategy**

Proposes hypotheses, competing explanations, and a bounded test plan. Suggestions remain proposals until translated into allowed wrapper IDs and validated by policy.

### Module 3 — Governance and authorization

**Kratos — Policy enforcement**

Maps a proposed action to deterministic policy checks and provides a decision reason. The enforcement engine is code with tests, not an LLM deciding whether its own plan seems acceptable.

**Exousia — Approval validation**

Validates approval identity, mission and scope binding, permitted operation, expiry, policy version, revocation, and replay resistance.

**Prytanie — Governance operations**

Maintains policy-version records, review queues, exception proposals, and decision history. It cannot waive a hard prohibition through free-form text.

**ERGOUSIARCH is the doctrine implemented here**, but it is not an additional autonomous authority or one of the 16 registry roles.

### Module 4 — Asset and environment understanding

**Aleph (alias: Alpha) — Asset/context intelligence**

Builds summaries from approved lab inventory, explicit scope, and evidence. Distinguishes observed assets from inferred relationships and does not expand scope.

**Arkeon — Environment stewardship**

Tracks lab readiness, snapshot/reset/cleanup status, expected network attachment, and environment consistency. A failed isolation check blocks execution rather than selecting a looser network.

### Module 5 — Mission design and execution

**Genesis (alias: Genarch) — Scenario and workflow specification**

Defines preconditions, expected outcomes, allowed steps, and cleanup requirements for repeatable lab scenarios. It cannot make arbitrary target changes.

**Protos — Bounded execution adapter**

Coordinates only registered tool wrappers under the Local Runner and records execution status/output references. It is not a general-purpose shell, exploit runner, or permission to execute arbitrary commands.

### Module 6 — Baseline, resource and priority

**Ab-Initio — Baseline establishment**

Captures initial lab/service state and baseline completeness before a test; it does not claim that the baseline is universally true outside its recorded time/scope.

**Dynas — Resource management**

Enforces or recommends per-mission runtime, output, request, concurrency, and storage budgets. Hard limits are enforced in code, not merely suggested.

**Primus — Priority arbitration**

Ranks task candidates according to owner intent, prerequisites, risk and budgets. Priority is not permission; a high-priority task remains blocked if policy denies it.

### Module 7 — Evidence and independent verification

**Mneme — Evidence memory and provenance**

Indexes mission history, manifests, reports, and evidence references. It must not invent missing evidence or remove contradictory records. Sensitive content is redacted before optional model use.

**Origo — Independent verifier**

Checks findings against evidence and an appropriately distinct method, assertion, or independent oracle. It can return `VERIFIED`, `REJECTED`, `DISPUTED`, `INCONCLUSIVE`, or `UNABLE_TO_VERIFY`. It must state the method, evidence references, and limitations. It cannot simply rephrase the originator's explanation.

## Shared typed task envelope

Every handoff should include:

- `mission_id`, `task_id`, parent task, named role/module, and current mission state.
- Objective, explicit target/asset references, permitted wrapper/action IDs, and exclusions.
- Input references with provenance and reality labels.
- Expected output schema and validation requirements.
- Deadline, timeout, output limit, retry policy and cancellation behavior.
- Required evidence, success criteria, and escalation path.
- Status: `PROPOSED`, `READY`, `BLOCKED`, `RUNNING`, `COMPLETED`, `FAILED`, `INCOMPLETE`, or `CANCELLED`.

## Role/authority matrix

| Activity | Propose | Decide/authorize | Execute | Verify |
|---|---|---|---|---|
| Interpret intent | Monarque | Owner clarifies | None | Contract validation |
| Coordinate plan | NEXARCH / Archon support | Owner approves mission; Gate validates | None | Workflow audit |
| Enforce scope/policy | Kratos module | Deterministic Gate + required owner approval | None | Policy tests/audit |
| Establish baseline | Ab-Initio | Gate authorizes its bounded read operation | Approved read-only wrapper | Independent comparison |
| Run lab tool | Melete/Genesis propose; NEXARCH schedules | Gate 1 and Gate 2 + owner approval | Local Runner / Protos adapter | Origo or other distinct check |
| Retain evidence | Mneme indexes | Storage policy and access controls | Evidence service | Hash and provenance validation |
| Verify finding | Origo | Verification contract, not the originator | Distinct bounded check where needed | Origo emits verdict, report preserves limitations |

## Implementation constraint

The first versions should be ordinary typed modules, not an agent swarm. The exact code boundaries can be refined during design review, but the authority boundaries above cannot be weakened. Model use remains undecided; deterministic fixtures and mocked outputs should cover tests even if an LLM is eventually introduced.
