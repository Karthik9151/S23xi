# Open Owner Decisions and Decision Log

**Status:** active register. Defaults below are planning assumptions, not hidden authorization to implement or deploy.

## Decisions derived from the uploaded questions image

| ID | Question | Current default | Status |
|---|---|---|---|
| D-001 | Build now or stay design-only? | Design documents only; no implementation until explicit C-1 approval | Selected by owner in screenshot |
| D-002 | Which real tools first? | Port/service discovery, HTTP probe/TLS check, conservative web baseline, and lab log/detection checks | Selected categories; exact versions/wrappers still open |
| D-003 | Add an AI model to the first version? | Decide later; first usable version must work without a model | Selected by owner |
| D-004 | Product and role names? | Keep Sentra as working title from the new draft and preserve NEXARCH, ERGOUSIARCH, Origo, and all 16 registered role names | Working default; public product branding needs final confirmation |

## Technical/operational decisions

| ID | Decision needed | Current recommendation | Must be decided by |
|---|---|---|---|
| D-005 | Auth method for one owner | Local owner bootstrap; server-side opaque session; password hash such as Argon2id if password login is chosen; HTTP-only cookie and CSRF protection; revocable CLI credential | S0/S1 |
| D-006 | Exact lab virtualization/networking | Disposable VM(s) or vetted isolated local setup with host-only networking and no default internet/home-LAN route | Before S3 |
| D-007 | First wrapper/tool versions and exact argument schemas | Pin after tool/license/OS compatibility review; Nmap, constrained HTTP client/curl, testssl.sh, ZAP baseline, local log/Sigma pipeline are candidates | S0/S3 |
| D-008 | Local evidence storage format and backup | Local raw files plus SHA-256 manifest and PostgreSQL metadata; stronger external digest anchoring deferred | S0/S4 |
| D-009 | Web and CLI authentication separation | Browser sessions for UI; separate scoped/revocable CLI credential; never copy session cookie into CLI | S0/S1 |
| D-010 | First owner bootstrap mechanism | A deliberate local command/console setup that can run only when no owner exists; disable after setup | S1 |
| D-011 | Temporary Render staging | Optional, only after current cost/card, origin/session/CSRF, data location, and security constraints are reviewed; no Runner hosting | S9 |
| D-012 | Later hosting | Compare AWS versus Cloudflare based on actual API/database architecture, cost and security; do not presume Cloudflare hosts every component | S11 |
| D-013 | AI provider/model/data policy | No model in the first version; any later provider needs privacy/cost decision and tests with mocks | S10 |
| D-014 | License and public contribution policy | Undecided; do not imply an open-source license or invite contributions until chosen | Before public release |
| D-015 | T4 real-world target work | Disabled and outside current stages; would require separate charter amendment and written engagement-specific authorization model | Not before separate owner review |
| D-016 | Local platform/OS for Runner | Choose and test the primary OS, process-group termination, route checks and secret-store behavior | Before S3 |
| D-017 | Retention/export/delete policy | Define retention, local backup, redaction, report export and removal of sensitive artifacts | Before real data capture in S4 |
| D-018 | Name clearance | Keep Sentra provisional; check potential product/name conflicts before public launch | Before public branding |

## Decision-record template

For any material change record:

- ID, date, owner, status (`PROPOSED`, `APPROVED`, `REJECTED`, `SUPERSEDED`).
- Problem and requirements.
- Options considered and why the alternatives were not selected.
- Security, authorization, privacy, cost and operational impact.
- How the choice will be tested and rolled back.
- Explicit approval. A discussion or generated plan is not approval.

## Non-decisions that must not be mistaken for approval

- This design package does not approve code, repository changes, scanning, lab provisioning, deployment, model-provider use, paid resources, or T4 work.
- Mentioning a tool does not install or authorize it.
- Choosing Render as a potential staging destination does not mean deployment is safe or free.
- Keeping a role name does not mean it must be a separate LLM.
- A future AWS/Cloudflare comparison is not an instruction to provision either provider.
