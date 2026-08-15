# Terminal projection v1

Pandaloco owns the task state machine and the only business-terminal compare-and-set operation. DSH session, turn, item, model, tool, process, checkpoint, or pull-request completion is non-authoritative evidence.

## States

```text
admitted -> leased -> running -> terminal_pending -> terminal_committed
                         |                |
                         +-> quarantined <-+
```

`terminal_pending` requires the current `(task_id, attempt_id, runtime_epoch, lease_token)` tuple, the selected route, all required owner receipts, and the final artifact references. The compare-and-set operation succeeds only when the attempt remains current and no terminal value exists.

`terminal_committed` rejects every later effect, receipt replacement, artifact-authority change, and terminal write. A late event is stored or discarded according to the Pandaloco quarantine policy and never changes the committed result.

Timeout first revokes the lease and stops or quarantines the complete worker process tree. Pandaloco reconciles receipts and effects before deciding a terminal value; a process exit alone is not failure or success authority.

Read-only or text-only tasks may use a lightweight receipt, but the receipt still binds the current task, attempt, and epoch and does not bypass the unique terminal compare-and-set operation.

## Evidence watermarks

Pandaloco persists an opaque, monotonically increasing `start_watermark` before dispatching the attempt. It advances an owner-store position for every admitted event, effect receipt, specialist receipt, process-state transition, and quarantine decision. A DSH sequence number may be stored as evidence but never replaces this Pandaloco-owned position.

The current lease holder may request `terminal_pending` only after the complete process tree is stopped or drained and Pandaloco seals an `end_watermark` at or after every accepted effect and receipt for that attempt. The terminal compare-and-set requires the current tuple and lease, the selected route, both persisted watermarks, `end_watermark >= start_watermark`, and a complete verification of only the records in that closed window. The tuple, watermarks, route, verified receipt ids, artifact digests, and proposed outcome are one atomic terminal input.

Events at or before another attempt's window, after the sealed end, from a revoked lease, or without a unique owner-store position are quarantined and cannot move either watermark or terminal state. Replay reuses the persisted window and deduplicates by owner-store event identity; it never widens a sealed window. On timeout, Pandaloco revokes the lease, kills or quarantines the complete process tree, records that transition, and seals the end only after the owner confirms no still-running descendant can emit an accepted event.

## Specialist receipt verification

Schema validity is necessary but not authoritative. Before a specialist receipt enters the evidence window, the Pandaloco-owned verifier must:

1. require exact equality between receipt `task_id`, `attempt_id`, and `runtime_epoch` and the current attempt tuple;
2. require `owner` to be in the attempt's capability-specific owner allowlist;
3. resolve `artifact_ref` through that owner, hash the returned bytes, and require equality with `content_digest_sha256`;
4. verify `verification.method` and `material_ref` over the complete canonical receipt, including `receipt_id`, tuple, owner, outcome, artifact reference, digest, and issue time;
5. atomically reserve `(owner, receipt_id)` in the Pandaloco receipt store and bind it to one attempt window;
6. accept an identical retransmission only as idempotent evidence with no new effect, and reject a conflicting, cross-attempt, cross-epoch, wrong-owner, stale-window, or already-bound replay.

The verifier persists its decision and owner-store position. Transcript text, model output, DSH durable events, or a schema-valid object without this contextual decision cannot support terminal projection.

## Terminal CAS input

```json
{
  "task_id": "task_opaque_01",
  "attempt_id": "attempt_opaque_01",
  "runtime_epoch": "dsh-epoch-0001",
  "lease_token": "lease_opaque_01",
  "selected_route": "dsh-epoch-0001",
  "start_watermark": "owner-position-000100",
  "end_watermark": "owner-position-000140",
  "verified_receipt_ids": ["receipt_opaque_01"],
  "artifact_digests_sha256": ["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
  "proposed_outcome": "completed"
}
```

Changing any tuple member, moving an event outside this window, presenting an unverified receipt id, or reusing the sealed window for another attempt rejects the compare-and-set.

## Outcome projection

DSH evidence may support `completed`, `quality_fail`, `abstained`, or `failed` owner outcomes. Pandaloco maps verified owner outcomes into the existing public closed status set; this contract introduces no public status.

## Rejected cases

- DSH returns final text and directly marks a task completed.
- A stale attempt commits after a retry acquired a new fence.
- A model statement replaces a specialist owner receipt.
- A schema-valid receipt names a different task, attempt, epoch, owner, artifact digest, or previously bound receipt id.
- Legacy and DSH routes both commit an effect or terminal value for one attempt.
- A late tool result mutates a terminal task.
