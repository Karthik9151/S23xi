# Sentra Threat Model

**Status:** proposed threat/control register. Rows are design requirements, not proof of effective controls. Each control must become a test or review artifact in the relevant stage.

## 1. Assets and trust boundaries

**Assets:** owner credentials and sessions; API tokens; mission/scope/approval records; local Runner configuration; lab credentials; raw tool output; evidence manifests and hashes; verification judgments; reports; audit records; PostgreSQL data; local lab targets; deployment secrets; software supply chain.

**Trust boundaries:** browser ↔ API; CLI ↔ API; control plane ↔ Local Runner; Runner ↔ lab network; tool output ↔ parser; tool/agent proposal ↔ Gate; evidence writer ↔ evidence reader; verifier ↔ originating execution component; local files ↔ hosted metadata; source control ↔ deployed secrets.

## 2. Threats and controls

| Threat | Example | Required control | Validation evidence |
|---|---|---|---|
| Unauthorized target contact | Bad scope or typo points scan at public IP | Two Gates, local allowlist, resolved-IP validation, T3 public-address rejection | Tests prove denial at Gate 1 and Gate 2 |
| Scope drift | Redirect or DNS result changes host | Revalidate every redirect and resolved address; bind approval to scope digest | Redirect/DNS mutation tests |
| Self-authorization | Conductor approves its own plan | Owner approval plus deterministic policy and independent Gate checks | Negative tests for absent/replayed/expired approval |
| Prompt injection | Tool output asks agent to scan another host | Treat all target content/output as untrusted data; model cannot choose executable or widen scope | Adversarial fixture cannot change allowed scope |
| Arbitrary command execution | Model text passed to a shell | Fixed executable path, typed wrapper schemas, argument arrays, no shell evaluation | Injection tests with metacharacters and unknown flags |
| Runner takeover | Hosted control plane tries to access host | Local Runner has no inbound internet listener; outbound-only coordination; local Gate | Network test shows no inbound path and no cloud-to-target route |
| Lab escape | Vulnerable VM reaches home network | Host-only isolation, route/firewall verification, no unsafe mounts, unprivileged execution | Lab isolation checklist and route tests |
| Unbounded scan | Too many hosts, requests or long runtime | Hard host/port/request/time/output/concurrency caps in wrappers | Limit tests and kill-switch tests |
| Incomplete stop | UI cancel marks run stopped while process continues | Local stop signal, process-group termination, terminal-state reconciliation | Live cancellation test checks process disappearance and evidence status |
| False positive | Tool banner is reported as vulnerability | Finding schema, evidence reference, distinct verification method, uncertainty field | A deliberately false claim is rejected by Origo |
| Verifier collusion | Verifier merely repeats originating explanation | Separate verifier contract/process boundary and distinct assertion/method | Tests where executor claim is contradicted by independent fixture |
| Evidence tampering | Local output edited after run | Hash manifests, read-only file permissions, append-only records where supported, rehash on read | Deliberate file alteration is detected |
| Hash/manifest rewrite together | Attacker edits output and its local manifest | Protect/anchor manifest digest separately where feasible; disclose that local hashes alone are not immutable | Threat-model review; no false claim of absolute immutability |
| Secret leakage | Cookies or tokens included in logs/reports | Redaction, header/body filtering, local-only raw evidence, secret scan, prompts excluded by default | Fixture with fake secret shows redacted report/logs |
| Session theft/CSRF | Stolen browser context submits run request | HTTP-only cookie, Secure/SameSite in HTTPS, CSRF tokens, origin checks, rotation/revocation, short expiry | Browser/API auth and CSRF test suite |
| CLI credential theft | Token stored in readable file or logs | Narrow revocable token, OS file permissions or secret store, no echo/logging | Permission and redaction tests |
| Single-user authorization gap | Guessable ID returns another record | Owner identity checked on every query/mutation even with one owner | Direct API object-ID tests |
| Public owner bootstrap | Anyone creates first owner on hosted app | Bootstrap only when no owner exists and via explicit local/console ceremony; disable endpoint afterward | Fresh install and re-bootstrap tests |
| SQL injection/data corruption | Invalid input reaches raw SQL | ORM/parameterized SQL, schema validation, migration review | API fuzz/negative tests and DB integrity checks |
| Agent failure/retry duplicate | Interrupted task starts twice | Idempotency keys, leases, typed state transitions, re-check Gate before retries | Fault-injection and concurrency tests |
| Model hallucination | Invented CVE or untrue fix claim | Model optional; evidence-backed reports and independent verifier | Unsupported claim remains hypothesis or is rejected |
| Dependency/supply-chain issue | Malicious or vulnerable scanner package | Pin versions, review tool origins/checksums, dependency audit, least privilege | Reproducible install and dependency scan record |
| Free-tier/billing surprise | Deployment begins charging or exposes data | $0 through S4; current terms reviewed; no paid resource without decision | Dated cost decision and billing settings review |
| Backup/data loss | PostgreSQL/local artifact loss | Backup and restore test, clear retention, local evidence backup policy | Recorded restore drill before hardening exit |
| Privacy creep | Lab captures unrelated home/production logs | Named sources, narrow time windows, local-only storage, retention/deletion policy | Redaction and source-allowlist tests |
| False assurance from deployment | App loads but security tests not run | Deployment status separated from acceptance status; CI and smoke evidence required | Stage report links exact test evidence |

## 3. STRIDE summary

- **Spoofing:** session theft, forged Runner result, forged approval. Controls: server-side sessions, action-bound approvals, Runner identity and audit linkage.
- **Tampering:** modified scope, tool output, manifest, policy or report. Controls: scope digests, versioned policy, hashes, append-only history, revalidation.
- **Repudiation:** owner denies approval or action. Controls: timestamped mission/action audit events, approval provenance, tool versions, request IDs.
- **Information disclosure:** raw logs reveal tokens or third-party data. Controls: local-only raw artifacts by default, redaction, least privilege, retention limits.
- **Denial of service:** runaway scan or runaway workflow. Controls: hard budgets, caps, timeouts, cancellation, local kill switch.
- **Elevation of privilege:** model gains shell or host access. Controls: fixed wrappers, no arbitrary shell, unprivileged runner, Gate enforcement and local isolation.

## 4. AI-specific risks if a model is later approved

A later model review should use the current OWASP Top 10 for LLM Applications rather than hard-coding an unverified version number here. Required controls include: prompt-injection resistance; output schema validation; no model authority; minimum data sharing; mocked tests; token/cost ceilings; provenance on retrieved knowledge; and tests proving malicious tool output cannot alter scope or invoke tools.

## 5. Security invariants

1. No action is run solely because an agent requested it.
2. No T3 target is contacted unless explicit scope and current approval pass both Gates.
3. The control plane has no route to a target and never runs scanner processes.
4. Failure or uncertainty cannot become an allow.
5. Every claimed finding points to evidence and has a verification status.
6. A failed verifier leaves the finding unverified/inconclusive, not silently promoted.
7. A cancel/kill event cannot be represented as a successful complete run.
8. Simulated, observed, inferred, and hypothesized records remain distinguishable.

## 6. Validation lifecycle

For each row, record state as `DESIGNED`, `IMPLEMENTED`, and `TESTED` separately. Add test name, environment, date, commit/revision, result, and known limitations. A design assertion or a passing unit test alone does not prove the entire deployed boundary works; high-risk controls require integration tests and lab validation.
