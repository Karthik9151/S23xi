# Evidence, Provenance, Verification, and Reports

**Status:** required data contract proposal. Raw output and real artifacts are not present in this design package.

## 1. Evidence model

Every execution produces one or more evidence records. An evidence record describes the artifact; it is not automatically proof that a vulnerability exists.

Required manifest fields:

- Evidence ID, mission ID, task ID, parent evidence IDs, and finding IDs where applicable.
- Reality label: `simulated`, `lab-observed`, `inferred`, `hypothesized`; reserve `real-observed` for a future separately authorized tier.
- Capture timestamp in UTC, Runner host/runtime version, tool name/version, wrapper version, and configuration/template ID.
- Approved target reference, approved scope digest, policy version, Gate 1 and Gate 2 decision references, and approval reference.
- Artifact relative path/local storage identity, media type, byte count, SHA-256 digest, exit status, redaction status, and parse/collection limitations.
- Provenance: who/what created it, source method, parser version, and any transformation steps.
- Status such as `RAW_CAPTURED`, `HASH_VERIFIED`, `PARSED`, `REDACTED`, `SUPERSEDED`, or `UNAVAILABLE`.

A missing field required by the relevant evidence type must be reported as incomplete. Never invent metadata to make a record appear complete.

## 2. Raw artifacts and integrity

- Raw output is stored locally by default in a controlled evidence directory excluded from version control.
- Finalize artifact bytes before computing SHA-256 and writing the manifest. Verify the hash whenever artifacts are retrieved or reports are rebuilt.
- Raw artifacts are treated as append-only. Corrections, reparsing and redaction produce new linked records rather than overwriting the original capture.
- Read-only file permissions and separate manifests reduce accidental changes, but do not make a local file cryptographically immutable against an owner or attacker who can rewrite both artifact and manifest. Stronger tamper evidence requires protecting/anchoring manifest digests separately; the design must state what is and is not protected.
- Raw outputs may contain credentials or personal data. Do not send them to an LLM or upload them to a hosted database by default. Store a redacted derivative and reference the local raw artifact when a shareable report is required.

## 3. Finding states

Each finding should move through explicit states, for example:

`HYPOTHESIS → EVIDENCE_COLLECTED → VERIFICATION_PENDING → VERIFIED | REJECTED | DISPUTED | INCONCLUSIVE | UNABLE_TO_VERIFY`

A status transition records who/what requested it, evidence references, verifier method, time, and reason. No agent may promote its own finding directly to `VERIFIED`.

- **Verified:** evidence and an independent check support the defined claim within the stated scope/time.
- **Rejected:** the verification check contradicts the claim or exposes a clear false positive.
- **Disputed:** credible evidence conflicts or the claim is only partly supported.
- **Inconclusive:** the check did not settle the claim.
- **Unable to verify:** prerequisites, access, or evidence were unavailable; this is not equivalent to rejected or verified.
- **Hypothesis/unverified:** a plausible claim without successful independent confirmation.

## 4. Independent verification

Origo receives the claim, verification plan, evidence references, and permissible read-only/re-test operations. It does not blindly trust the producing tool's narrative. Where possible, it uses a distinct method, assertion, protocol interaction or known lab-side oracle. The test must be safe, within the same approved scope, and separately permitted by the Gate if it touches the target. Origo records the method and its independence limitations. When no appropriate second method exists, the report says that verification was not independent enough and leaves the claim unverified.

Examples: cross-check a discovered service against a bounded connection check or target-side lab inventory; independently inspect a certificate with a separate parser; replay a known benign test event and check a specific log/detection assertion. Re-running the same scanner with identical settings is not automatically independent verification.

## 5. Report contract

Reports should include:

1. Mission objective and owner-approved scope, exclusions, environment and reality labels.
2. Execution date/time, tools/versions, wrapper profile, policy and approval references.
3. Summary of verified findings and their evidence references.
4. Separate tables for rejected claims, disputed findings, unverified hypotheses and inconclusive checks.
5. Severity rationale and CWE/CVE/CVSS references only when supported, plus confidence and limitations.
6. Reproduction steps that remain within the authorized scope, expected/observed result, impact explanation and remediation.
7. Detection/defense observations, log coverage, missing telemetry and possible false positives.
8. Failed, blocked, cancelled or partially completed tasks, including why they stopped.
9. Evidence manifest hashes and instructions for local integrity verification.

No report may label a simulated result as lab-observed, or an inferred claim as directly observed. A mission with some verified items and some failed steps must retain that partial status.

## 6. Retention and privacy

Define retention, cleanup, export, backup, and deletion before the system captures real lab data. Separate raw from redacted evidence. Secret values are not copied into reports. A future hosted deployment must explicitly decide which metadata can leave the local machine and must not upload raw evidence without owner-approved redaction and transfer policy.
