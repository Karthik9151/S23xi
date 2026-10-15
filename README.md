Sentra — Personal Cybersecurity Research Companion
Working name: Sentra (provisional; final product-name clearance is an owner decision).
Document status: Fresh design baseline only. No implementation is authorized yet.
Mission: Perform real, authorized cybersecurity checks against the owner's own isolated lab; preserve traceable evidence; independently verify findings; and include defensive detection work.

Authority rule: Human intent is not authorization. AI output is not authority. A tool runs only when the mission, written scope, approval, policy, local environment, and stop conditions all pass deterministic checks.

1. The idea in plain language
Sentra is a single-user workspace for planning and documenting authorized security research. You describe what you want to learn. Sentra turns that request into a mission draft, asks about missing details, and writes a narrow scope describing exactly which lab assets and actions are permitted. NEXARCH coordinates the work but cannot approve its own plan. The deterministic ERGOUSIARCH Gate checks the mission in the control plane; a second Gate on your own machine checks the local scope and lab state before the Local Runner can invoke an allowlisted tool. Evidence is preserved with hashes and provenance. Origo independently re-tests claims and the report separates verified results from hypotheses, disputed results, and inconclusive checks.

The hosted service never scans targets. Real tools run through a Local Runner on your machine, against assets explicitly listed in your isolated lab scope.

2. Product identity and what is being combined
Sentra is the platform, single-user workspace, mission history, evidence catalogue, CLI, and optional React interface.
NEXARCH is the one primary Conductor. It interprets the approved mission plan, orders tasks, and consolidates results; it cannot grant itself authorization.
ERGOUSIARCH is the governance doctrine implemented in deterministic policy, approval, audit, and Gate services—not a free-form AI persona.
Origo is the independent verifier. It can return verified, rejected, disputed, incomplete, or inconclusive outcomes.
The 16 named roles from the NEXORION agent registry are retained and organized into seven implementation modules. They are initially bounded software roles/modules, not sixteen separate LLMs.
Reused from NEXORION
The design retains NEXARCH, ERGOUSIARCH, Origo, the 16-role vocabulary, explicit mission contracts, bounded handoffs, evidence provenance, authorization at the execution boundary, and fail-closed behavior.

Deliberately changed
Sentra is single-owner and CLI-first, includes real tools against the owner's own isolated lab at T3, gives defensive detection its own pillar, uses a local Runner with an independent Gate, and defines a new roadmap in docs/STAGES.md. NEXORION's old Stage 1–5 sequence and synthetic-world-first product scope are not carried forward. Simulation/replay remains available for testing and training, but it must never be mislabeled as real observation.

3. User-selected defaults
The uploaded decisions screenshot is recorded as follows:

Keep this at design-documents-only status until explicit implementation approval.
Plan the first real-tool categories as port/service discovery, HTTP/TLS checks, a web vulnerability baseline, and log/detection checks—all against the approved lab only.
Decide about an AI model later. The first useful version must work without a model.
Preserve the previous names for the existing named roles. Sentra remains the working platform name in these documents; public branding is not yet final.
4. Main lifecycle
Intent: capture the question, assumptions, desired result, and unknowns.
Scope: create a versioned mission contract with exact targets, exclusions, allowed tool wrappers, limits, deadlines, and stop conditions.
Plan and approve: NEXARCH prepares a plan; the owner explicitly approves the bounded T3 mission.
Gate 1: the control-plane Gate validates the mission, approval, policy version, requested tool, limits, and target scope.
Gate 2: the local Gate independently validates a locally controlled scope file, lab-network identity, target resolution, tool wrapper, expiry, and kill-switch state.
Execute locally: the Local Runner invokes only the approved wrapper and records exact tool/version/configuration metadata.
Evidence: raw outputs remain local by default; hashes, timestamps, tool versions, scope and mission references, and reality labels accompany the records.
Verify: Origo evaluates a finding using a distinct check or independent assertion. A repeated explanation is not verification.
Report: verified findings, rejected claims, unverified hypotheses, limitations, and remediation are separated clearly.
