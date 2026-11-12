# 0016. A repo's rating lives on its follow; a document's source is computed

- **Status:** Proposed
- **Date:** 2026-11-12

## Context

- PublicTxt-Pages and obsidian-publictxt-facets are about to facet on the repo a document came from, and on how far its reader trusts that repo.
- Forks already name the follow they came through (`fork_remote`, ADR-0012, ADR-0015), and follows are documents with front matter (ADR-0014).
- Both tools already read `rating`, an integer 1–5, as a document's rating.
- PublicTxt-Pages' spec names "a subscriber-declared tier in subscription config" as the honest v1 shape for trust. There is no data source yet for anyone else's ratings (IDEAS.md: consensus and reputation).
- Copying a repo's rating into each fork would be derived data on disk (ADR-0003). It would also go stale whenever the rating changes.

## Decision

- A follow's `rating` (1–5) is this author's rating of the followed repo (spec §3.2). No new key: a follow is a document about a repo, so its rating is the repo's.
- A document's source and source rating are computed when reading, never written (spec §5.4). A fork is joined to its follow by `fork_remote`, or by `fork_repo` when it has no remote name. That is the rule obsidian-publictxt-remotes uses.
- A document that isn't a fork is from this repo and has no source rating. Views keep it apart from unrated sources.
- `source_repo` stays a separate, open question (spec §5): it names the repo a document is published from, not where its text came from.

## Consequences

- + One edit to a follow re-rates everything from that repo. Nothing to keep in step.
- + Works in a single published repo: Pages has the follows and forks it needs to compute both.
- + Reuses the rating Pages and Facets already show, filter and sort on.
- − Each consumer computes the source and the join itself until the Document model carries them (spec §13).
- − Renaming a follow drops its forks' source rating until their `fork_remote` is updated.
- − A follow is itself rated, so a rating facet over all documents counts follows as rated pages. That is accurate, but views may want to show follows apart.
- ! Reputation from other people (aggregated, weighted, consensus) is deferred. It would add to this author's own rating, not replace it.
