# Branch and review policy

Every adoption change starts from an immutable upstream commit in a dedicated worktree and candidate branch. Review inputs identify the upstream commit and tree, the fork tree, the complete downstream path manifest, and every resolved configuration or artifact identity relevant to the requested phase.

`upstream` is fetch-only and has a disabled push URL. A candidate never follows `main`, `master`, `latest`, a mutable tag, or an unpinned package range as runtime authority.

## Extension order

Changes use the first sufficient mechanism in this order: profile or configuration, Pandaloco facade or gateway, external adapter, DSH native plugin, bounded core patch. A core patch records its reason, upstream base, affected paths, replacement or removal condition, validation, and replay status in [`epochs/patch-ledger.yaml`](epochs/patch-ledger.yaml).

## Review inputs

- The candidate base and tree match the declared epoch.
- The diff is limited to the accepted write set.
- Source, configuration, artifact, SBOM, license, patch, gate, and rollback identities are complete for any epoch proposed for execution.
- The current and candidate epochs have separate roots and can coexist for fixture, replay, security, capacity, and rollback checks.
- Missing evidence is red; a reviewer never converts absence, skipped work, or an unsupported platform into a pass.

## History and remote operations

Shared history is not rewritten across epochs. A rejected candidate remains evidence or is replaced by a new candidate identity. Commit, merge, push, pull request creation, branch protection, release, and deployment each require authority outside this document.

