# Research Pillars

Sentra starts with a small chain that yields a useful, defensible report, then adds more capability only when each preceding safety gate is proven. Governance, evidence, and verification are cross-cutting requirements for every pillar.

## Pillars and sequence

| ID | Pillar | Scope | Stage |
|---|---|---|---|
| P1 | Lab attack-surface inventory | Port/service inventory on scoped lab hosts | S4 |
| P2 | HTTP and TLS posture | Bounded HTTP behavior, redirects, headers, certificate and protocol checks | S5 |
| P3 | Web vulnerability baseline | Conservative OWASP ZAP baseline/passive assessment against designated training app | S6 |
| P4 | Detection and defense | Log collection, detection rule evaluation, alert coverage, false positives and telemetry gaps | S7 |
| P5 | Evidence, verification and reporting | Provenance, hashes, finding normalization, independent verification, honest report | Cross-cutting from S4 |
| P6 | Governance and execution safety | Scope, approvals, dual Gates, local isolation, runner, kill switch and audit | S1–S4 and every later stage |
| P7 | AI assistance and learning | Optional later intake/planning assistance, adversarial testing, owner revision loop | S10 decision; learning ongoing |

## P1 — Lab attack-surface inventory

**Goal:** enumerate approved lab services and produce a reproducible inventory. Candidate tool: Nmap via a constrained wrapper. Output includes host/port/service observations, tool version, target identity, timestamp, scope digest, and limitations. Do not infer a vulnerability from a service banner alone.

**Study:** TCP/IP, ports, routing, DNS, service fingerprinting, scope validation, scan safety and rate limits.

## P2 — HTTP and TLS posture

**Goal:** measure selected web/TLS properties without escaping scope. Candidate tools: a constrained HTTP client (such as `curl`) and `testssl.sh` or a reviewed equivalent. Restrict destinations, redirects, request count, bytes, runtime and concurrency. Redact cookies, auth headers and sensitive bodies.

**Study:** HTTP methods/status, redirects, cookies, security headers, TLS versions/ciphers, certificate chains and hostname validation.

## P3 — Web vulnerability baseline

**Goal:** identify common, non-destructive baseline issues in a deliberately designated training application. Start with the OWASP ZAP baseline/passive profile. Active scanning, exploitation, credential attacks and destructive checks are not implied and need a later separate lab-only decision.

**Study:** OWASP Top 10, baseline versus active scanning, false positives, finding evidence, CWE and remediation quality.

## P4 — Detection and defense

**Goal:** evaluate whether lab activity is visible to the defensive controls. Ingest only named log sources and selected time windows. Validate parsers and detection rules against known test events. Use Sigma-compatible content where useful, while recording rule versions and local engine/configuration. Report positive detections, missed detections, false positives, missing telemetry and retention gaps.

**Study:** event logs, detection engineering, Sigma concepts, alert quality, SIEM pipelines, time synchronization and incident response.

A lack of a log is a limitation of observation, not proof that an event did not happen. Defensive reports must include source coverage and collection gaps.

## P5 — Evidence, verification and reporting

Each finding has a stable identifier, claim, severity rationale, confidence/verification state, affected in-scope asset, reproduction method, evidence references, limitations and remediation. Origo selects an appropriately distinct test or assertion and may reject the claim. Reports include a separate unverified section. A scanner severity is not automatically a verified security impact.

**Frameworks:** CVSS where applicable; CWE; CVE only when the mapping is supported; a clear remediation rationale; OWASP/PTES terminology where helpful.

## P6 — Governance and execution safety

Every wrapper call is governed by the mission's targets/actions/budgets and current approval. Gate 1 and Gate 2 must independently allow. Tests include unknown/out-of-scope targets, expired scope, DNS/redirect changes, malformed wrappers, timeouts, cancellation, interruption and kill-switch actions. Hard prohibitions are implemented in code and cannot be overridden by an agent.

## P7 — AI assistance and learning

The first version works without an AI model. If later approved, use it only for clarification, prioritization suggestions, explanations and draft hypotheses. It cannot authorize, run arbitrary tools, change scope, decide that its own output is verified, or transmit raw evidence by default. Evaluate model benefit against deterministic templates and mocks; record cost/privacy decisions.

Learning is continuous: every lab mission should produce a private review note with the goal, scope, tools/versions, outcome, lessons, evidence limitations and next study topic. Do not place secrets or sensitive target data in public write-ups.

## Framework alignment

| Area | Useful reference vocabulary | How Sentra uses it |
|---|---|---|
| Recon/inventory | MITRE ATT&CK Reconnaissance/Discovery concepts | Label observed inventory; do not infer attacker activity from a service list |
| Web/API | OWASP Top 10 and OWASP API Security Top 10 | Organize baseline checks and remediation; scanning remains scoped to lab |
| Threat modeling | STRIDE | Organize security risks and controls in `THREAT_MODEL.md` |
| Engagement lifecycle | PTES pre-engagement, intelligence gathering, vulnerability analysis, reporting | Emphasize authorization, scope, analysis and reporting; exploitation/post-exploitation is outside the initial build |
| Finding quality | CWE, CVE, CVSS | Map only when supported; record uncertainty and limitations |
| Detection | Sigma and ATT&CK data-source/detection concepts | Evaluate lab event visibility and rule behavior rather than claiming full SOC coverage |
| AI safety, if adopted | Current OWASP Top 10 for LLM Applications | Recheck current version during S10; don't freeze stale numbering into enforcement logic |

## Scope exclusions

The first chain does not include external target discovery, mass Internet scanning, brute forcing, denial-of-service, stealth/evasion, persistence, malware, exploit chaining, data theft, or automated post-exploitation. A future owner request cannot silently expand these exclusions; it requires a separate documented decision and safety review.
