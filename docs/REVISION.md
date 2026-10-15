# Learning and Revision Plan

Use this alongside the stages. It is a study roadmap, not a rigid calendar. Mark a subject complete only when you can explain it and use it safely without relying on copied commands.

## Learning sequence

| Order | Topic | Supports | Teach-back target |
|---|---|---|---|
| 0 | Charter, scope and authorization, computer-misuse law relevant to your jurisdiction | Every stage | Explain why tool access is not authorization and how a deny decision is recorded |
| 1 | TCP/IP, subnets, routing, DNS, host-only networking and firewalls | S0/S3/S4 | Draw the lab path and demonstrate why an out-of-scope IP cannot be reached |
| 2 | Linux users/permissions, processes, signals, filesystem security and Python subprocess safety | S3 | Explain fixed executable paths, argument arrays, timeouts and process-group termination |
| 3 | HTTP, redirects, cookies, TLS and certificate validation | S5 | Trace a request and explain how redirect/DNS checks keep it inside scope |
| 4 | Port/service inventory and how to interpret tool output | S4 | Distinguish an observed open port from an inferred vulnerability |
| 5 | STRIDE, authorization binding, state machines and security testing | S1–S3 | Describe each trust boundary and the test that proves its control |
| 6 | PostgreSQL, SQLAlchemy, Alembic, transactions and concurrency | S1/S2/S8 | Explain migration safety and why SQLite-only tests are insufficient |
| 7 | Browser sessions, cookies, CSRF, CORS, owner checks and CLI secrets | S1 | Explain session rotation/revocation and why CSRF applies to cookie-authenticated requests |
| 8 | Hashes, manifests, provenance, evidence integrity and report uncertainty | S4 onward | Recompute a hash, detect an altered artifact, and state hash limitations honestly |
| 9 | CWE/CVE/CVSS and evidence-backed report writing | S4 onward | Write a finding with impact, evidence, verification method, limitations and remediation |
| 10 | OWASP Top 10, passive web scanning, false positives and safe lab methodology | S6 | Explain baseline versus active scan and how to keep checks non-destructive |
| 11 | Log sources, detection rule evaluation, Sigma concepts and incident response | S7 | Demonstrate an expected alert and a controlled missed-detection test in the lab |
| 12 | Model security, prompt injection, structured outputs and excessive agency | Only if D-013 is approved | Show malicious tool output cannot change scope or call a wrapper |
| 13 | Deployment security, secrets, backups, recovery and cost controls | S9/S11 | Explain the control-plane/Runner split and restore from a backup |

## Practical habits

After each authorized lab run, write a private review containing: objective, scope and exclusions, exact tools/versions, approval ID, Gate decisions, result, evidence paths/hashes, independent verification method, stop/cleanup status, unexpected events, limitations, and what you learned. Keep raw logs and credentials private. Do not publish a write-up containing target identifiers or sensitive artifacts without reviewing authorization and redaction.

## Learning completion standard

A topic is ready to move from notes to project acceptance when you can:

1. Explain its security boundary in your own words.
2. Identify at least one likely failure mode.
3. Describe a negative test that proves the guard works.
4. Explain what the available evidence does not prove.

The aim is a safe, reproducible research practice—not to maximize the number of tools or autonomous roles.
