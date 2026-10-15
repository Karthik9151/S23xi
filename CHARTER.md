
Sentra Charter
Status: owner-review draft. Design only; no code, repo changes, scans, or deployments are authorized.
Working platform name: Sentra. Existing role names NEXARCH, ERGOUSIARCH, and Origo are retained. Public branding is still an owner decision.

1. Purpose
A personal, single-user cybersecurity research companion for learning, carrying out controlled experiments in the owner's isolated lab, assessing owned systems, and producing evidence-backed, independently verified reports. It combines a CLI, a React workspace, a FastAPI control plane, and a Local Runner that remains on the owner's machine.

2. User, workspace, and allowed environments
Exactly one owner and one logical workspace in the initial design.
No public registration, organization tenancy, external sharing, or multi-user authorization features in the first version.
Targets must be owned lab assets or explicitly authorized targets listed in a written scope. The initial T3 implementation is limited to the owner's isolated lab.
Any future real-world engagement tier requires a separate, explicit design and written decision; it is not implied by T3.
3. Non-goals
Sentra is not an autonomous attacker, public scanning service, botnet, persistence framework, malware-development platform, or mechanism to test third parties without authorization. It does not replace owner judgment. It does not require an LLM to operate. It does not claim that a finding is true merely because an agent produced it. It does not begin with exploitation or post-exploitation workflows.

4. Binding rules
ID	Rule
C-1	No implementation, code, repository changes, scans, or deployments until the owner explicitly approves the implementation phase. This work package remains design-only.
C-2	Written scope first. Every target and permitted action must be in a versioned mission scope before any tool touches it.
C-3	Lab by default. T3 operates only against the owner's explicitly scoped, isolated lab. T4 is disabled and outside the initial implementation scope.
C-4	Two gates. Both the control-plane Gate and the independently implemented local Gate must allow an action.
C-5	Models and agents are untrusted. Their outputs are proposals; structured outputs must be validated. No raw model text is executed as shell, SQL, Python, or arbitrary tool arguments.
C-6	One Conductor, no self-approval. NEXARCH is the sole primary Conductor. No component can authorize its own proposed action, widen its scope, or verify its own result.
C-7	Fail closed. Missing, expired, contradictory, malformed, or ambiguous scope/approval/policy/environment data blocks execution.
C-8	Evidence-bound reporting. A finding without supporting evidence remains a hypothesis. A finding without a successful independent check cannot be labeled verified.
C-9	Reality labels are mandatory. Use simulated, lab-observed, inferred, hypothesized, and, only after a future separate decision, real-observed. Labels must not be blended.
C-10	Secrets stay out of source control, prompts, routine logs, reports, screenshots, and unredacted hosted metadata. Raw sensitive artifacts remain local by default.
C-11	The kill switch wins. It blocks new work and terminates active wrapper process groups as safely and promptly as the operating system permits; interrupted runs are recorded as interrupted, never successful.
C-12	Responsible handling. A real-system finding is handled only within its written authorization and disclosure rules. No unsolicited testing or disclosure of third-party data.
C-13	The Local Runner is not inbound reachable from the internet. If remote control-plane integration is later added, the Runner initiates outbound polling and still validates locally.
C-14	Defense is a first-class pillar. Detection, logs, telemetry validation, and defensive control checks are planned alongside authorized assessment.
C-15	Single-user does not mean unauthenticated. Web sessions, CSRF defenses, CLI credential handling, owner checks, expiry, revocation, and audit are required before any hosted exposure; MFA is required before the control plane is reachable from the internet.
C-16	No duplicate orchestrator. Archon is subordinate coordination support under NEXARCH, not a second Conductor. ERGOUSIARCH is enforced by deterministic modules, not a persona's judgment.
C-17	Do not silently inherit NEXORION's stages. The authoritative new roadmap is docs/STAGES.md; earlier NEXORION phase numbers are historical only.
C-18	Budget control. Spending remains $0 through S4. Before any paid resource or service is used, record price, limits, security impact, data location, exit plan, and explicit owner approval.
C-19	Preserve the prototype. Review prototype/ against this design; automation may not rewrite it without separate explicit permission.
C-20	Status claims require evidence. “Implemented” means code exists and tests prove behavior; “tested” means a recorded test actually ran. A document or checklist is not runtime proof.
5. Autonomy tiers
