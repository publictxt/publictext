# 0015. Obsidian PublicTxt Remotes: named remotes, local sources, desktop only for good

- **Status:** Proposed
- **Date:** 2026-11-10, recording decisions taken in obsidian-publictxt-remotes

## Context

- The follow-and-fork plugin of ADR-0011 was named obsidian-publictxt. Beside obsidian-publictxt-facets, the name didn't say which half it is.
- People want to follow repos that aren't hosted: another vault on the same computer, or a folder synced from someone else.
- The plugin's checks and fork provenance stand on git history: "what's new" is a diff between commits, and `fork_blob` is the merge base for updating a fork later. A plain folder has neither.
- ADR-0011 left mobile as a later phase over a host's HTTPS API.
- Follows are published content (ADR-0014), and a repo's files are public even when a page isn't (spec §5.3). A local folder's path written into a follow, or into a fork's `fork_repo`, would be published with it.

## Decision

- The plugin is **PublicTxt Remotes**: id `publictxt-remotes`, repo [publictxt/obsidian-publictxt-remotes](https://github.com/publictxt/obsidian-publictxt-remotes). Remotes are repos and vaults elsewhere; a local one is the special case.
- Remotes are named, as `git remote` names a URL. A follow's name is its file name (spec §3.2); a fork records it as `fork_remote` (spec §5.2). A hosted fork keeps `fork_repo` beside it; the name is read-only once forks record it.
- A local source is a git repo on this computer. Its follow says `local: true` and holds no path; the plugin keeps the folder in its own settings, per computer. It is cloned and checked like a hosted one, so it shows only what is committed. Its forks have `fork_remote` and no `fork_repo`.
- A plain folder is not a source. The plugin makes the commits for the user instead, only when asked: **Snapshot** commits everything changed in the folder (nothing if nothing changed), on the branch the source follows and no other; **Make repo** runs `git init` with a `.gitignore` that leaves out a vault's app folders, then snapshots.
- Desktop only, for good. Local sources, snapshots and making repos need a file system and git, which mobile doesn't have. Supersedes ADR-0011's mobile phase.
- No path or `file:` URL is written into a note. They are not canonical repo URLs (spec §3.1).

## Consequences

- (+) Another vault, or any folder, can be followed and forked from, with the same checks and provenance as a hosted repo.
- (+) One code path: local and hosted sources differ only in where git fetches from.
- (+) Forks say which follow they came through, readably, for hosted and local sources alike.
- (+) A local source's follow and its forks can be published and synced: no path in them.
- (-) A local source sees nothing until it is committed: a snapshot is a deliberate step.
- (-) A fork from a local source names a follow that can't be fetched elsewhere: its provenance is only complete for the author.
- (-) Each computer sets a local source's folder once. Until then the source shows as not set up there.
- (-) Renaming a follow would orphan its forks' `fork_remote`, so names are read-only. A link that Obsidian updates on rename is a possible way out, parked.
- (-) The plugin writes outside the vault (snapshot, making a repo), always on the user's action, into the source's own folder. Obsidian's developer policies require this to be disclosed in the plugin's README; it is.
- (!) Obsidian Sync may carry the plugin's settings, and with them a folder set on one computer, to another where it doesn't exist. A check there reports the folder missing.
