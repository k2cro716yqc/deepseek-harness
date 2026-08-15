# Agent Note: Pandaloco adoption control plane

Status: proposed

English | [中文](2026-08-15-pandaloco-adoption-control-plane.zh.md)

## Problem

Pandaloco needs DeepSeek Harness capabilities without transferring public task, provider, effect, deployment, or business-terminal authority to the harness. The upstream project is pre-release and permits breaking source, configuration, wire, persistence, and plugin changes, so a package-only dependency or an untracked local patch set cannot identify what ran or make upgrades reproducible.

Pandaloco already operates General and Research agents, a Model Plane, monitoring, controlled physical deployment, and domain-owned effects. A DSH integration that bypasses those services would create a second public task path, duplicate provider governance, permit two authoritative effect paths during shadowing, or treat harness completion as a business result.

DSH filesystem and tool controls are also insufficient as host isolation. They do not by themselves confine the full process tree, network, host reads, devices, or ambient credentials.

## Proposal

Pandaloco will maintain a tracked fork and identify every executable DSH release as an immutable runtime epoch containing exact upstream and fork source, downstream delta, resolved configuration, tested artifact, provenance, SBOM, license, patch ledger, gate matrix, storage policy, and rollback identity. Candidate and current epochs will coexist for validation; floating or overwrite upgrades are invalid.

Static gate claims will use a two-level identity. A candidate-subject manifest excludes gate results and the final distribution manifest, each G0-G5 result binds that stable subject and its evidence, and the final distribution manifest covers the results. A static PASS without that versioned binding is invalid.

DSH will remain an internal execution engine behind the existing Pandaloco facade. An internal selector will bind one admitted task attempt to exactly one legacy or DSH epoch for its lifetime. Pandaloco will retain admission, authentication, attempt fencing, tool and specialist authorization, receipts, artifacts, and the unique business-terminal compare-and-set operation.

Production DSH model calls will use the Model Plane `/model-tasks` interface. Provider credentials, profiles, routing, fallback, egress, error normalization, latency, and usage will remain Model Plane responsibilities. DSH will map cognitive work to model tasks without loading a direct provider in a production epoch.

V1 will run one active task attempt in one Linux DSH worker process with private roots, a clean environment, an explicit policy, and one fail-closed outer authority over the complete process tree and auxiliary executables. DSH policy will remain an inner filesystem and tool guard. A missing or unverifiable outer sandbox will reject execution.

Legacy and DSH runtimes will coexist until a separately reviewed retirement decision. Default shadow evaluation will replay recorded inputs without effects. Live dual execution will require an explicit worker, GPU, model-residency, concurrency, time, cost, and effect-suppression budget, and only one route may issue authoritative effects or terminal state.

Metrics will use the existing Prometheus and Grafana path with bounded labels. Per-attempt identifiers will remain in Pandaloco-owned structured evidence rather than metric labels. Centralized logging will remain disabled until the logging owner admits the exact DSH source, and default telemetry will exclude prompts, model outputs, tool payloads, secrets, and credentials.

Physical registry, service onboarding, and deploy, cutover, and rollback actions will remain owned by the physical control, agent-services, and deploy-ops repositories. The DSH fork will carry deployable identity and adoption rules but will not activate itself.

## Alternatives considered

**Consume only published DSH packages.** Rejected because package APIs do not preserve the exact upstream source, configuration, local adaptation, and patch replay information required to audit breaking pre-release upgrades.

**Fork and modify DSH freely.** Rejected because an unbounded downstream codebase would make upstream changes expensive to absorb and would obscure which patches can be replaced or removed.

**Replace the existing Agent Plane with DSH.** Rejected because the current Pandaloco facade, task authority, Model Plane, monitoring, deployment controls, and domain receipts already carry responsibilities that DSH does not replace.

**Let DSH call model providers directly.** Rejected because it would duplicate credential, routing, fallback, egress, error, and usage governance and would bypass current resource controls.

**Use DSH filesystem policy as the complete sandbox.** Rejected because it does not provide whole-process-tree host, network, syscall, device, secret, and host-read isolation.

**Run live legacy and DSH shadows by default.** Rejected because it doubles model and tool pressure and can create duplicate effects unless capacity and effect suppression are proven first.

**Upgrade a shared runtime in place.** Rejected because existing attempts would lose immutable runtime identity and rollback would depend on mutable filesystem and persistence state.

## Acceptance criteria

- The fork contains the versioned adoption files, candidate-subject and distribution manifests, schema-valid G0-G5 results, and complete English, Chinese, and pairing-record triplets.
- Machine-readable rules reject direct production provider access, a parallel public API, ambiguous runtime selection, multiple authoritative effect paths, unowned deployment, unadmitted logs, high-cardinality metric labels, and unbudgeted live shadow execution.
- Epoch identity covers source, configuration, artifact, provenance, supply chain, patches, storage, gates, and rollback without claiming absent evidence.
- Physical owner and dependency records keep incomplete service onboarding, capacity, watch health, centralized logging, outer sandbox, and runtime evidence red.
- Static validation can reproduce the manifest and every structural pass or failure without running DSH.

## Risks

The control documents add maintenance work and can drift from source or physical owner contracts if upgrades skip their review. Static validation cannot prove runtime isolation, provider routing, effect suppression, performance, or recovery. Per-task processes and side-by-side epochs consume more memory, storage, GPU headroom, and operational effort than a shared in-place runtime. The proposal accepts those costs to preserve task isolation, reproducibility, bounded fork drift, and rollback.
