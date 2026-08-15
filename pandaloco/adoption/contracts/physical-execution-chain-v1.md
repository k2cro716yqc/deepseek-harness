# Physical execution chain v1

The public edge terminates at the existing Pandaloco facade for the admitted task family. DSH wire, session, process, and runtime epoch identifiers remain internal and create no public task endpoint or port.

## Required order

```text
existing Pandaloco facade
  -> internal runtime selector
  -> one legacy or DSH runtime epoch
  -> Model Plane and allowlisted tool or specialist brokers
  -> owner receipts and artifacts
  -> Pandaloco terminal compare-and-set
```

Production DSH model requests use the logical Model Plane `/model-tasks` interface. Model provider credentials, profiles, selection, fallback, egress proxy or tunnel, error normalization, latency, and usage remain Model Plane responsibilities. DSH owns only the internal mapping from a cognitive step to a model task.

Tools and specialist services remain behind Pandaloco authorization. DSH may retain a safe receipt reference in runtime evidence, but a transcript or plugin cannot mint, replace, or reinterpret the owner receipt.

## Physical owners

- `phys-center-control` owns the physical contract, registry requirements, network exposure rules, and readiness composition.
- `phys-agent-services` owns the concrete DSH service manifest, health contract, degradation behavior, and onboarding evidence; it remains unavailable until it has a versioned owner artifact.
- `phys-deploy-ops` owns controlled deployment, cutover, rollback execution, and state reconciliation.
- `phys-model-plane` owns model execution governance and provider egress.
- `phys-observability-stack` owns metrics, dashboards, alerts, and monitoring runbooks.

The DSH fork owns source, artifact, epoch, and adoption metadata. It does not own registry admission, port publication, traffic activation, physical secrets, deployment, rollback execution, monitoring truth, domain effects, or business terminal state.

## Failure behavior

A missing owner, contract digest, readiness result, capacity value, sandbox result, or source admission blocks the dependent edge. Implementations do not fall back from the Model Plane to a direct DSH provider, from centralized logs to an ungoverned sink, or from the current facade to a new public DSH endpoint.
