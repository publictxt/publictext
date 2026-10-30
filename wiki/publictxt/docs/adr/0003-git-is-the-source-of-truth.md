# 0003. Git is the only source of truth; derived data is never committed

- **Status:** Proposed
- **Date:** 2026-10-13

## Context

- The README layout commits `indexes/` and `tags/` folders.
- Generated files in Git go stale, conflict on merges and add noise to every diff.
- The project already prefers view-time reconciliation to write-time normalization (ADR-0006). Committed indexes contradict that.

## Decision

- A repository holds authored content and the manifest only.
- Databases, indexes, tag pages and search data are projections. Each is keyed by commit SHA and can be deleted and rebuilt at any time.
- Projections update incrementally from `git diff --name-only <old>..<new>`.

## Consequences

- + No merge conflicts on generated files, and nothing on disk can drift out of sync.
- + The local database is a cache, so "delete it and resync" is always safe.
- − Every consumer computes its own projections. This is cheap at the expected scale of text repos with thousands of files.
- ! Known conflict: `resource_id` in front matter is derived from the `bookmark` URL. A change to URL normalization would make every stored value stale. Spec §8.2 lists the options; resolve this before 0.1 is final.
- ! The same question applies to `author` and `source_repo` in front matter (spec §5).
