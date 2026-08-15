# Pandaloco adoption control

English | [中文](README.zh.md)

This directory is the versioned source for Pandaloco's DeepSeek Harness adoption rules. It identifies immutable runtime epochs, assigns cross-repository owners, defines internal execution obligations, and records the evidence required before a later phase may run.

The documents do not configure or launch a runtime. A missing or red requirement remains blocked; no document in this directory authorizes model calls, tools, host writes, deployment, or public API changes.

## Authority

[`authority-manifest.yaml`](authority-manifest.yaml) names the accepted decisions, exact source baseline, write set, and authorization ceiling. [`CANDIDATE.sha256`](CANDIDATE.sha256) fixes the normative review subject; the [G0 result](gates/results/g0-result-v1.json) and the other G1-G5 results bind its digest, and [`MANIFEST.sha256`](MANIFEST.sha256) fixes the complete distribution after those results exist. [`epochs/epoch-0001.yaml`](epochs/epoch-0001.yaml) records the initial zero-source-delta epoch without claiming that an artifact, configuration, or runtime has been tested.

The Pandaloco task service retains admission, authentication, attempt fencing, receipts, artifacts, and business terminal state. Harness session, turn, item, model, process, checkpoint, or pull-request completion is evidence only.

## Execution path

The required path is the existing Pandaloco facade, an internal runtime selector, one legacy or DSH epoch, the Pandaloco-owned model and tool brokers, and the Pandaloco terminal compare-and-set operation. Production DSH model calls terminate at the Model Plane `/model-tasks` interface; DSH does not own provider credentials, routing, fallback, egress, or usage accounting.

V1 assigns one active task attempt to one Linux DSH worker process with isolated roots, environment, policy, and a fail-closed outer sandbox over the complete process tree. The DSH filesystem and tool policy is an inner guard and is not host, process, network, device, or secret isolation.

## Review order

1. Read [`authority-manifest.yaml`](authority-manifest.yaml) and the two source audits.
2. Read the epoch, attempt, terminal, receipt, persistence, physical execution, runtime selection, observability, and platform rules.
3. Evaluate [`gates/gate-matrix-v1.yaml`](gates/gate-matrix-v1.yaml), validate each claimed G0-G5 result starting with the [G0 result](gates/results/g0-result-v1.json) against [`gates/gate-result-v1.schema.json`](gates/gate-result-v1.schema.json), and reproduce its candidate and evidence digests.
4. Verify ownership with [`ownership/cross-repo-dag-v1.yaml`](ownership/cross-repo-dag-v1.yaml).
5. Use the upgrade and rollback documents only after all dependencies for the requested phase pass.

## Negative guarantees

- DSH exposes no parallel public task API through this adoption layer.
- A task attempt never follows a floating branch, tag, version, configuration, or runtime selection.
- Legacy and DSH execution never produce two authoritative effect or terminal paths for one attempt.
- Metrics exclude task, attempt, process, worker, and receipt identifiers from labels; centralized logs remain disabled until their owner admits the exact source.
- macOS host-native write or executable-tool operation is unsupported, and AP-03 sandbox reuse remains unavailable.
