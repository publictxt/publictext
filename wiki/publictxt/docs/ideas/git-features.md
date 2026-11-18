# Git features

Parked ideas for using more of Git than storage and history. None is planned; the [IDEAS.md](../IDEAS.md) rows say when to look again.

| Idea | What | Where it stands |
|---|---|---|
| Topic branches | Branches named by a shared standard, so a reader can follow one aspect of a repo | Rejected: follow whole repos and filter when reading ([ADR-0005](../adr/0005-follow-repos-filter-at-query-time.md)) |
| Page versions | Compare a page across repos, or across its own history | Partly built: Obsidian PublicTxt Remotes compares a fork with its source by blob SHA (Fork update) |
| User view | Everything one person has contributed, across the repos you follow; or their version of a page you have, and how it differs from yours | Follows are per repo and one author per repo ([ADR-0009](../adr/0009-single-author-per-repo.md)), so a person's view is their follow's pages. Cross-repo, not yet |
| Git namespaces | Git's ref namespaces: several repos' refs in one repository | Idea only |
| Git-based wikis and wiki bridges | Other Git-backed wikis as sources, bridged to PublicTxt | Idea only; [WikiTool](../../projects/WikiTool.md) converts between wiki formats |
| Git-based public projects | Projects run in the open from a Git repo, with funding for them | Idea only |
