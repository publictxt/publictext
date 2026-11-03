# 0012. A fork records its provenance in its own front matter

- **Status:** Proposed
- **Date:** 2026-11-03, recording the decision taken in obsidian-publictxt

## Context

- Forking a page means copying it from another repo into your own. Later you want to know whether the source changed, update your copy, or propose your changes back.
- That needs the source repo, path, branch and the exact version copied, kept somewhere.
- A separate list of forks (a database, a sidecar file) goes stale when a note is moved, renamed or deleted, and is a second source of truth.
- ADR-0003 bans derived data on disk.

## Decision

- A forked document carries `fork_repo`, `fork_path`, `fork_branch`, `fork_commit`, `fork_blob` and `fork_date` in its front matter (spec §5.2). Flat keys, so Obsidian Properties shows and edits them.
- The list of forks is derived by scanning documents, never stored.
- `fork_blob`, the source file's git blob SHA at fork time, is the change marker and the merge base.
- Prefix `fork_`: `upstream_` is git's word for a remote, and `source_` collides with the open `source_repo` key (spec §5), which means the repo a document lives in.

## Consequences

- + One source of truth. Moving, renaming or deleting a fork needs no bookkeeping.
- + Provenance travels with the page, including into other people's repos when they fork a fork.
- + Keeping the merge base makes a three-way "update my fork" and "propose upstream" possible later.
- + Consistent with ADR-0003: provenance is authored, not derived. It records a choice made at a point in time and can't be recomputed from the repo.
- − Six reserved keys in front matter. Writers must keep them intact when editing.
- − Detecting local edits to a fork needs a hash of the body without the `fork_*` keys. Deferred (spec §13).
