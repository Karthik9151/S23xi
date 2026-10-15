# CLI Workflow (Proposed Interface)

**Status:** command names are illustrative and are not executable yet. This is a design contract, not an installed CLI.

## Why CLI-first

The CLI is the dependable operator interface for local scope review, Runner preflight, kill-switch control, evidence verification, and reproducible mission execution. The React UI is a complementary view for mission editing, approvals, progress, timelines, evidence, and reports; it cannot create a bypass around the CLI or Gate controls.

## Proposed command groups

Exact spelling can be settled in S0, but behavior should follow these boundaries:

```text
sentra status
sentra mission draft
sentra mission validate <mission-file>
sentra scope show <scope-file>
sentra scope validate <scope-file>
sentra approval review <mission-id>
sentra gate explain <mission-id>
sentra runner preflight <mission-id>
sentra runner status
sentra runner stop
sentra mission run <mission-id>
sentra evidence verify <manifest-path>
sentra report build <mission-id>
sentra audit show <mission-id>
```

These examples describe desired user-facing operations only. There is no promise that a command or option currently exists.

## Command safety rules

- `mission draft` creates a proposal, not authorization.
- `mission validate` checks structure and semantics but does not run a tool.
- `scope validate` must reject malformed, stale, broad, ambiguous or unauthorized target definitions.
- `approval review` displays the exact mission, target scope, wrapper IDs, limits and expiry before a human can approve it.
- `gate explain` is read-only and explains a deterministic decision; it cannot convert a deny into an allow.
- `runner preflight` checks isolation and local policy without running scanners.
- `runner stop` is local, available offline, and should remain usable when the API is unavailable.
- `mission run` fails unless both Gates return allow and the owner-approved mission has not changed.
- `evidence verify` recomputes hashes and checks manifest references; it does not claim that the represented finding is true merely because the file hash matches.
- `report build` separates verified, rejected, disputed, unverified and inconclusive findings.

## CLI credentials

CLI authentication is distinct from browser sessions. Use revocable credentials, least privilege, no credential echo, restrictive local file permissions or the OS credential store, and redaction in tracebacks/logs. Avoid copying browser cookies into scripts. Local-only operations such as the kill switch must still work if the hosted API or network is down.

## Exit codes and machine-readable output

Use stable non-zero exit codes for validation failure, authorization denial, environment/preflight failure, execution failure, cancellation and verification failure. A human-readable message is helpful, but automation should rely on a typed JSON result with `status`, `reason_code`, mission/task ID, and evidence references. Never return exit code 0 for blocked or interrupted execution. Final values are a design item for S0.
