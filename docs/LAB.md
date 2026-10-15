# Local Lab and Local Runner

**Status:** requirements only. No lab is provisioned and no scan is authorized by this document.

## 1. Objective

Provide a disposable and isolated environment where Sentra can run real security tools against assets the owner controls. The control plane organizes work; only the local Runner can invoke lab tools, and only after both Gates approve the exact action.

## 2. Tiers

- **T0:** inspect existing notes/logs/reports read-only.
- **T1:** create plans and scope proposals; no target contact.
- **T2:** deterministic simulation/replay with no live network access.
- **T3:** real tools against the owner's isolated lab with explicit per-mission approval and both Gates.
- **T4:** real-world engagements are disabled and excluded from the initial build plan.

T3 does not authorize general-purpose penetration testing. Each action must be listed in a narrow allowlist; the MVP excludes credential spraying/brute force, denial of service, persistence, destructive changes, exploit chains, stealth/evasion, and data exfiltration.

## 3. Isolation requirements

The lab design should use disposable virtual machines or appropriately isolated local containers/VMs, with a dedicated host-only/private lab network. Before T3 is enabled, the owner must validate:

- Lab targets have no bridge to the public internet or home/corporate LAN unless a separately documented requirement is reviewed; initial default is no such bridge.
- The host firewall and route table confirm the intended boundary.
- The Runner has no route to unlisted networks for the approved T3 profile.
- There are no shared folders, clipboard, host credentials, Docker socket, or privileged host mounts exposed to vulnerable targets unless separately justified and documented.
- Targets are owned or intentionally vulnerable training appliances; only synthetic credentials/data are present.
- Snapshots/reset and cleanup work; resource caps prevent a lab VM or scan from exhausting the host.
- The lab target inventory, network interface, and expected address ranges are stored locally and validated before each run.
- Outbound access and DNS are denied by default for the execution path; any required update path is separate from test execution.

If any isolation preflight is uncertain, Gate 2 blocks the run.

## 4. Local Runner requirements

- Runs on the owner's machine. No hosted Runner and no target connections initiated by Render, AWS, or Cloudflare.
- Accepts only typed, authenticated job envelopes and reads a local scope configuration that the control plane cannot rewrite.
- Validates mission ID, scope digest, approval expiry, target, allowed wrapper, argument schema, tool version, runtime/output/request limits, and kill switch before each wrapper call.
- Uses fixed executable paths and argument arrays; never evaluates AI text as shell or code; no arbitrary executable or arbitrary flags.
- Applies per-wrapper timeouts, output size limits, connection limits, rate limits and a concurrency ceiling.
- Prevents redirects, DNS rebinding, IP changes, or discovery of a new host from becoming implicit scope expansion.
- Runs as a dedicated unprivileged account; `sudo` is not available to wrappers by default.
- Records start/end time, wrapper/tool version, scope/policy digest, command template ID, exit status, redacted stdout/stderr references, and cancellation/failure state.
- Does not report success until the run has a terminal status and expected evidence has been finalized.

## 5. Kill switch and stop conditions

A local `STOP` file or equivalent owner-accessible control must prevent new wrapper launches and request interruption of active process groups. The owner must be able to stop a run without the cloud service. Stop on scope change, approval expiry, target identity mismatch, forbidden redirect, lab network mismatch, resource limit, tool crash, control-plane disconnect beyond the agreed lease, critical telemetry failure, or any unexpected destructive behavior. Stopped/interrupted runs retain partial evidence and a non-success status.

The exact shutdown signal and cleanup approach must be designed/tested on the selected operating system. “Kill switch exists” is not acceptance evidence until a running test proves it stops work.

## 6. First approved tool families

The screenshot chooses these categories. The names below are proposed candidates to review and pin—not a statement that they are installed or approved for every target.

| Category | Proposed candidate | Initial profile | Restrictions |
|---|---|---|---|
| Port/service discovery | Nmap | Narrow TCP connect/service inventory against explicitly listed lab hosts/ports | No broad Internet ranges; rate/concurrency and duration caps; no OS-detection or script options unless separately approved |
| HTTP probe | Constrained `curl` wrapper or equivalent typed HTTP client | Known in-scope lab URL, bounded methods, headers, redirects, response bytes and timeout | No arbitrary URL; redirects cannot leave scope; redact cookies and authorization headers |
| TLS configuration | `testssl.sh` or reviewed equivalent | TLS/certificate posture of an approved lab endpoint | Target must resolve to scoped lab IP; bounded duration; output redaction and tool version captured |
| Web vulnerability baseline | OWASP ZAP baseline/passive profile | Designated training web app | Passive/baseline profile first; unrestricted active scans, credential attacks and destructive rules are excluded |
| Logs and detections | Read-only local log collector; Sigma-compatible local rule evaluation where useful | Import or inspect records from named lab sources | Read-only collection; label gaps; avoid collecting production/home data; rules and parser versions recorded |

Tool selection, exact executable versions, flags, target examples and wrapper schemas are decided during S0/S3 design review. Do not paste raw tool commands into an agent prompt and execute them.

## 7. Target validation

Every target is resolved and checked immediately before execution. The local scope identifies explicit hostnames/IPs and, where justified, narrow approved CIDRs and ports. Block loopback/host addresses if they are not the target, public routable addresses in the T3 profile, link-local/service metadata addresses, multicast, broadcast, unknown/private subnets, and any resolved IP not in the local allowlist. DNS answers and HTTP redirects are revalidated at each hop. “Private IP” by itself is not proof of ownership or scope.

## 8. Upstream documentation to review

These are official project/documentation references for the proposed tool candidates. They are references only—not permission to run a tool, a wrapper specification, or proof that a version has been selected. Pin and review versions/licenses during S0/S3.

- [Nmap Reference Guide](https://nmap.org/book/)
- [curl tool documentation](https://curl.se/docs/tooldocs.html)
- [testssl.sh project](https://github.com/testssl/testssl.sh)
- [OWASP ZAP Baseline Scan](https://www.zaproxy.org/docs/docker/baseline-scan/)
- [Sigma CLI project](https://github.com/SigmaHQ/sigma-cli)
- [Render free deployment limits](https://render.com/docs/free)

## 9. Lab acceptance checklist

Before S4, record evidence for: network boundary test; target allow/deny test; unknown-host denial; DNS/redirect escape denial; user and process privileges; no arbitrary shell path; kill-switch behavior; timeout and output caps; scope expiry; runner disconnect; snapshot reset; and absence of secrets/real personal data in fixtures. Lab setup is an owner-controlled task after implementation authorization.
