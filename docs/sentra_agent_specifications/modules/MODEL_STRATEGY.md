# Model Strategy for the 16 Sentra Roles

**Status:** Design only. Provider and model selection remain undecided.

## Principles
1. Do not begin with 16 separate LLMs; role boundaries do not require separate models.
2. Deterministic code handles policy, approvals, state transitions, quotas, schema validation, evidence hashing, and process supervision.
3. Use models only for language interpretation, summarization, and hypothesis generation when they provide measurable value.
4. Treat every model response as untrusted; validate schemas and apply deterministic checks.
5. The system remains usable without a configured model.
6. Do not expose secrets, cookies, private keys, or unnecessary sensitive artifacts to models.
7. Record provider/model/version and prompt/template version for model-assisted results.
8. Do not describe model confidence as probability unless calibrated.
9. No billable model API until the owner approves a cost decision.

## Recommended strategy
| Role | Default | Optional model use |
|---|---|---|
| NEXARCH | Deterministic workflow | Plan/result summaries |
| Monarque | Typed intake and validation | Intent interpretation |
| Archon | Dependency tracker | Blocker summaries |
| Dynas | Quotas and measurements | Advisory estimates |
| Kratos | Deterministic policy engine | Explain reason codes |
| Exousia | Deterministic approval validation | None |
| Prytanie | Governance records | Review packet summary |
| Aleph | Parsers and graph correlation | Relationship summaries/candidates |
| Genesis | Approved templates | Test-spec drafts |
| Primus | Deterministic scoring | Advisory rationale |
| Ab-Initio | Collectors and manifests | Baseline summaries |
| Origo | Independent assertions/checkers | Evidence classification assistance, never self-certification |
| Mneme | Canonical retrieval/provenance | Optional semantic retrieval |
| Melete | Evidence-linked templates | Competing hypotheses |
| Arkeon | Deterministic lab lifecycle | None |
| Protos | Wrapper registry and supervisor | Suggest wrapper ID only |

## Enablement gate
Before enabling a model, evaluate schema validity, unsupported claims, scope drift, sensitive-data leakage, repeatability where required, latency, and cost. Test prompt injection in target artifacts and tool output. Failure must produce a structured error or manual path, never weaker governance.

## First release
Build typed contracts and deterministic workflows first. Use mocked model adapters in tests. Real provider use requires a separate owner decision covering privacy, data handling, cost, latency, and evaluation.
