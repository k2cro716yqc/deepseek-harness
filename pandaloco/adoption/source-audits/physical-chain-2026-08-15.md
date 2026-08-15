# Physical chain snapshot: 2026-08-15

This dated audit records non-sensitive observations and owner references used to define the adoption rules. It is not a service registry, deployment manifest, capacity promise, or substitute for a fresh readiness check.

## Observed services

- The local Model Plane gateway and Ollama runtime reported healthy.
- The General Agent, Research Agent, and research workload runtime reported healthy.
- Prometheus, Grafana, DCGM exporter, node exporter, cAdvisor, and blackbox exporter were running.
- PostgreSQL, Qdrant, Temporal, MinIO, and image-service components were running as supporting services.
- The model runtime observer timer was active.
- The latest consumer observability watch invocation failed with `SERVICE_DATA_PLANE_COMMAND_NOT_CONFIGURED`.
- No live Loki container was observed, and the centralized-log control package had not admitted a real DSH source.

## Owner references

- `phys-center-control` commit `191a20c3658ae360c9a65b1764209f1c63713e49`, paths `codex/architecture/physical-center-contract.md`, `codex/architecture/physical-center-runtime-boundaries.md`, and `codex/contracts/model-agent-boundary.md`, owns the physical-center contract, runtime ownership declarations, and the Model Plane and Agent Plane split.
- `phys-model-plane` commit `0d3860929a1bf82f3bb7afb4ba62168c03660b10`, path `codex/README.md`, owns provider profiles, model routing, fallback, egress, errors, latency, and usage.
- `phys-observability-stack` commit `ec2e2a03b475a41ecea825e254546c6012c0ac6d`, path `codex/README.md`, owns Prometheus, Grafana, exporters, dashboards, alerts, and runbooks.
- `phys-deploy-ops` commit `9d1e23514cba6b30f7fc9ea3ec84f95d36b09801`, path `codex/README.md`, owns controlled operational execution.
- `phys-agent-services` is an unversioned onboarding shell at this snapshot and cannot satisfy a live DSH service-admission dependency.
- `pdlc-multiagent-local` commit `44678a1472080f43f91fb3296e7bbe3aacabaf0c`, paths `codex/contracts/general-agent/api-contract.md` and `codex/contracts/ops-tool-use-policy.md`, owns the current General and Research runtime contracts; its working tree state is not an adoption input.

## Required interpretation

The healthy observations do not authorize DSH deployment. The failed watch, missing real log-source admission, unversioned agent-service owner, and absent worker and model capacity budget remain red live-readiness inputs.
