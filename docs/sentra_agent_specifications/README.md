# Sentra — Individual Agent Design Specifications

**Status:** Design only. This package does not authorize implementation.

This package defines the 16 named roles retained from the NEXORION concept for Sentra's authorization-first, real-lab architecture. Names are bounded responsibilities; they do not imply 16 separate AI models.

## Architecture invariants
- Sentra is the platform, single-owner workspace, mission history, CLI/UI, and evidence record.
- NEXARCH is the sole Conductor.
- ERGOUSIARCH is deterministic governance implemented through Gate services, not a persona.
- Origo is the independent verifier.
- Local Runner executes approved wrappers only in the owner's isolated lab.
- Hosted control plane never connects directly to targets.
- Agent/model output is untrusted and never authorization.
- Control-plane and local Gates independently check scope, policy, approval, expiry, environment identity, limits, and stop conditions.
- Unclear or unavailable conditions fail closed.
- Evidence reality labels remain distinct; corrections are new linked records.

## Roles
NEXARCH, Monarque, Archon, Dynas, Kratos, Exousia, Prytanie, Aleph, Genesis, Primus, Ab-Initio, Origo, Mneme, Melete, Arkeon, Protos.

## Files
- `agents/INDEX.md` — navigation.
- `agents/` — 16 detailed specifications.
- `modules/INTERFACES_AND_HANDOFFS.md` — shared contracts and authority order.
- `modules/MODEL_STRATEGY.md` — model use by role.
- `modules/ACCEPTANCE_MATRIX.md` — integration tests and release gates.

## Implementation distinction
Deterministic services should own policy, approvals, schemas, quotas, state transitions, evidence integrity, and process supervision. AI may be introduced later for bounded interpretation or hypotheses. Tool execution belongs to Local Runner, not a general-purpose model or hosted API.

Sentra remains a working name pending name/trademark checks.
