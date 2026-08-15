# Observability v1

The existing physical observability stack owns DSH service and runtime metrics. Monitoring provides health, resource, latency, error, and routing evidence; it does not authorize execution, promotion, effects, or business terminal state.

## Metrics

Prometheus labels are limited to bounded categories such as active epoch, runtime route, operation, result class, and failure class. `task_id`, `attempt_id`, session, process, worker, artifact, tool-call, and receipt identifiers are forbidden labels.

Per-attempt correlation belongs in Pandaloco-owned structured runtime evidence. That evidence may contain task, attempt, epoch, safe worker identity, and safe tool or effect receipt references according to the owning data classification.

## Sensitive content

Prompts, model outputs, tool payloads, files, secrets, credentials, authorization material, and raw owner artifacts are not metric labels or default log fields. An owner may admit a minimized field only through a separate data-classification and logging decision.

## Centralized logs

Centralized DSH emission is disabled until the logging owner admits the exact source, field allowlist, retention, transport, authentication, redaction, failure behavior, and evidence destination. Missing admission is a hard failure and cannot select an ungoverned local or remote fallback sink.

## Live readiness

The requested live route must prove current Model Plane, facade, worker, sandbox, Prometheus, Grafana, required exporter, watch, capacity, and deployment-owner readiness. The dated `SERVICE_DATA_PLANE_COMMAND_NOT_CONFIGURED` observation remains red until its owner closes it or supplies reviewed evidence that the selected route does not depend on that watch.

