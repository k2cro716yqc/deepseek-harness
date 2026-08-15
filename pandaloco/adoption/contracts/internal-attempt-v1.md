# Internal attempt v1

An internal attempt identifies one execution of an already admitted Pandaloco task. It does not deduplicate public task creation and does not add a field to any public request or response.

## Required fields

| Field | Requirement |
|---|---|
| `task_id` | Opaque Pandaloco task identifier. |
| `attempt_id` | Opaque identifier unique within the task. |
| `runtime_epoch` | Immutable reviewed epoch selected before execution. |
| `lease_token` | Fencing value held by the current writer. |
| `created_at` | RFC 3339 UTC timestamp issued by Pandaloco. |
| `state` | `admitted`, `leased`, `running`, `terminal_pending`, `terminal_committed`, or `quarantined`. |

The tuple `(task_id, attempt_id, runtime_epoch)` is immutable. A lease renewal may replace `lease_token` only through the Pandaloco attempt authority; a DSH session, model, tool, process, or transcript cannot mint or renew it.

## Valid example

```json
{
  "task_id": "task_opaque_01",
  "attempt_id": "attempt_opaque_01",
  "runtime_epoch": "dsh-epoch-0001",
  "lease_token": "fence_opaque_01",
  "created_at": "2026-08-15T09:00:00Z",
  "state": "admitted"
}
```

## Rejected examples

- Two public submissions sharing this internal tuple are not deduplicated; public admission owns duplicate handling.
- A running attempt that changes `runtime_epoch` is quarantined.
- An attempt without a current fencing value cannot issue effects or terminal state.
- A second live writer for the same attempt is rejected even if it reuses the DSH session identifier.

