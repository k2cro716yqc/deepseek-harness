# Runtime selection v1

The Pandaloco attempt authority selects one approved runtime route when it creates an internal attempt. The selection records `task_id`, `attempt_id`, `runtime_kind`, and `runtime_epoch` and remains immutable until the attempt reaches a Pandaloco terminal or quarantine state.

`runtime_kind` is `legacy` or `dsh`. A DSH value requires a reviewed immutable epoch; a legacy value requires the current legacy deployment identity. Zero routes, multiple routes, a floating route, or a mid-attempt route change is invalid.

## Authoritative path

Exactly one selected route may issue authoritative tool requests, owner-receipt references, artifacts, effects, or terminal evidence for an attempt. A comparison route is always non-authoritative and cannot acquire an effect-capable credential or writer lease.

## Shadow modes

| Mode | Model execution | Tools and effects | Authority | Entry requirement |
|---|---|---|---|---|
| `recorded-replay` | Replays accepted recorded inputs | None | None | Privacy and fixture admission |
| `live-single-route` | Selected route only | Selected route only | Selected route | Runtime gates and capacity |
| `live-dual-route` | Legacy and DSH | Selected route only; the non-selected route is effect-suppressed | Selected route only | Separate worker, GPU, model-residency, concurrency, time, cost, privacy, and suppression evidence |

`recorded-replay` is the default comparison mode. `live-dual-route` remains blocked when any budget or suppression proof is absent.

## Rollback

Rollback changes selection for new attempts to a known-good epoch. Existing attempts remain bound to their selected epoch and are drained, killed, or quarantined under the persistence and terminal rules; they are never resumed through another epoch.
