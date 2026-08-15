# Epoch persistence v1

Each runtime epoch has a private persistence root, and each attempt has a private descendant root. A task attempt is bound to its creation epoch; no process resumes or writes the attempt through another epoch.

## Writer ownership

One live writer lease exists per session. Every durable write carries the task, attempt, epoch, and fence. A stale or second writer is rejected before it can replace state.

DSH session data is runtime evidence and model-visible context. It is not the Pandaloco task record, owner receipt store, artifact authority, or business terminal record.

## Rollback order

1. Stop selecting the candidate epoch for new attempts.
2. Drain candidate attempts that remain within their approved deadline.
3. Revoke leases and kill or quarantine the complete process tree for attempts that cannot drain safely.
4. Reconcile owner receipts, effects, artifacts, and terminal state under the Pandaloco authority.
5. Select the known-good epoch for new attempts.
6. Clean candidate roots only after process and writer quiescence and the retention requirement have both passed.

Rollback never changes an active attempt's epoch and never rewrites another epoch's event or session history.

## Rejected cases

- Current and candidate epochs share a writable root.
- Two processes hold a live writer lease for one session.
- A rollback overwrites candidate state with old-epoch files.
- Cleanup begins while an executable descendant or writer remains live.
