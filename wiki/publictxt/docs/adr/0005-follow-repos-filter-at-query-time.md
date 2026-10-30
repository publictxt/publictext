# 0005. Follow whole repos and filter at query time; no topic branches

- **Status:** Accepted (subscription model). Proposed (removing topic branches).
- **Date:** 2026-10-13, recording an earlier decision

## Context

- del.icio.us offered per-tag subscriptions cheaply because it was centralized. In a Git model, fetching per tag (for example, sparse checkout per tag) is expensive and fragile.
- The README proposes topic branches with shared naming standards, so readers can follow one aspect of a repo. Branches diverge, the author has to keep merging them, and every author has to agree on branch names.

## Decision

- Readers follow whole repositories, listed under `follows` in their manifest.
- Filtering happens at query time, through saved filters.
- Published content lives on the default branch. Other branches are for drafts and pull requests.

## Consequences

- + One fetch per followed repo, and no coordination between authors on branch names.
- + The follow list is plaintext, so the social graph is public text too.
- − Readers fetch more than they read. That's fine for text. Repos with large media need guidance, such as partial clone (`--filter=blob:none`) or LFS.
- − The `follows` filter grammar is still open (spec §13).
