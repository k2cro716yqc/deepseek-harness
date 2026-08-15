# DSH epoch upgrade runbook v1

This runbook defines evidence and ordering. It does not authorize fetch, build, test, traffic, deployment, or remote Git operations.

## Prepare the candidate

1. Resolve the requested upstream ref to an immutable commit and tree in a clean isolated worktree.
2. Record the upstream comparison, dependency and license changes, configuration migration, wire and persistence changes, and affected DSH extension points.
3. Replay each active downstream patch in ledger order, or record reviewed upstream replacement or retirement evidence.
4. Resolve the complete runtime configuration without ambient environment fallback and compute its digest.
5. Build a candidate artifact from the fork whenever a core patch exists; record artifact identity, digest, provenance, SBOM, license digest, and executable closure.
6. Allocate separate session and attempt roots and create a rollback manifest that names the current known-good epoch.

## Validate side by side

1. Run static and schema checks for the exact candidate bytes.
2. Run keyless and no-tool lifecycle checks only after their separate authorization.
3. Run the reviewed Linux whole-process-tree sandbox suite before any model, MCP, shell, tool, workspace-write, or executable path.
4. Run Model Plane-only, tool and receipt, persistence, terminal, timeout, late-event, privacy, metrics, and degradation checks.
5. Replay accepted fixtures through current and candidate epochs without effects.
6. Before any live dual route, prove worker, GPU, model-residency, concurrency, time, cost, privacy, effect-suppression, and rollback headroom.
7. Complete versioned agent-service onboarding and a physical deploy, cutover, rollback, and state-reconciliation drill.

## Promote

The release authority verifies every required gate result, candidate and current artifact identity, capacity evidence, owner approval, and rollback target. New attempts may select the candidate only after that verification. Existing attempts retain their creation epoch.

## Fail and recover

A source, patch, configuration, artifact, schema, security, capacity, owner, readiness, or evidence mismatch blocks promotion. Stop selecting the candidate, drain or quarantine its attempts, reconcile effects and terminal state, and select the known-good epoch for new attempts. Do not overwrite roots, mutate running attempt identities, or rebuild under the same epoch identity.

