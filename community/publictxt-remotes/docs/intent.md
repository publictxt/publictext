---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/intent.md
mirror_branch: master
mirror_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
mirror_blob: d72c57136206a06ab558a7a525146f21de6f0c92
mirror_date: 2026-11-25T22:45:40+02:00
---
# Intent

## Why

PublicTxt's v1 aim is to "pull selected pages from other people's PublicTxt repos into a local
aggregate view" (publictext `README.md`). `obsidian-publictxt-facets` is the browsing half and
stays offline. This plugin is the pulling half: it talks to remote repos and writes into the
vault, only when the user asks. Outside the vault it writes only to a local source's folder,
again only when asked.

Use case: follow a few repos; see what changed since I last looked; keep some pages at hand;
make some my own; see when those change upstream.

## Decisions

- **Follow, mirror, fork.** Reading someone's notes and owning a copy stay apart. *Follow* a
  repo (a note in `follows/`). *Mirror* pages worth having at hand: upstream's copies under
  `community/<source>/`, overwritten by updates, not for editing. *Fork* the ones to make your
  own: your note, wherever you put it; upstream changes come through Compare. A file can't be
  both: forking a mirror moves it.
- **Notes carry their own facts.** Sources are follow notes (publictext ADR-0014); mirrors and
  forks carry `mirror_*` / `fork_*` provenance. All lists of them are derived by scanning, never
  stored: one source of truth, and moving, renaming or deleting needs no bookkeeping.
  Provenance is authored (it can't be recomputed), so it doesn't break publictext ADR-0003. The
  keys, not the folder, say what a note is, so a published note says so too.
- **Config, state and per-computer data are separate.** Sources are notes (public).
  `data.json` holds settings, a safe-to-delete cache, and per-computer paths, which are private
  and meaningless elsewhere, so never in a note.
- **Change detection by git blob SHA**: content-addressed and the same on every host. Stored at
  fork time it is also the merge base, should merging come.
- **Desktop only, git CLI.** Any host, the user's own auth, no rate limits, full history,
  `git merge-file` later; matches publictext ADR-0004. Local sources and snapshots need a file
  system anyway. Replaced a GitHub REST adapter, parked at tag `archive/github-adapter`.
- **One bare partial clone per source** in a hidden `.clones/` (blob-less: trees up front,
  contents on demand). Hidden: not indexed, skipped by Sync. Bare: no remote note shows up.
- **Follow whole repos, narrow locally** (publictext ADR-0005): `include` folder prefixes, the
  manifest's convention (`types`, `ignore`), not a new glob grammar.
- **Remotes are named**: the follow note's file name, recorded as `fork_remote` /
  `mirror_remote`. Hosted copies keep `*_repo` too, so a published copy leads to the original.
  Names are read-only, since copies record them.
- **Local sources are git repos.** A folder is followed through the same clone and check path.
  Without commits there's no "what's new" and no merge base, so the plugin offers to make the
  commits (**Snapshot**, **make a repo**).
- **A hosted source can have a local copy**: still one source, since two notes would split its
  copies and show twice on a site.
- **Updating a fork is a compare, not a merge.** Overwrite, write alongside as a new copy, or
  keep mine. `fork_seen` records what was dealt with, so `fork_blob` stays the merge base.
- **A fork's links change form, not target.** They are written as the vault writes links, paths
  kept; where they go is the wiki way. Pointing broken ones at notes of the vault is asked for.
  Neither is an edit. The plugin never changes a mirror's links: it is upstream's text.
- **The community folder is named for readers**: it's a section of the published site, and
  publictxt-pages uses "community" for where pages came from.

## Open questions

1. **Canonical repo URL** (publictext spec §13 q2): v0 rule in
   [features/sources.md](../../../wiki/publictxt/projects/obsidian-plugins/publictxt-remotes/features/docs/features/publictxt-remotes%20plugin%20-%20sources.md); propose it upstream.
2. **Removing a source leaves its clone.** Delete on remove (asking), a clean-up command, or
   leave it (re-adding is cheap). Left for now.
3. **`fork_remote` as a link** (`[[follows/name]]`): clickable and rename-safe if Obsidian
   updates links in properties; then names needn't be read-only. Needs checking.
4. **Sync carries `data.json`**: a `localPaths` folder set on one computer shows on another,
   where it's likely missing. A check reports it; not otherwise handled.

## Later

Merge a fork (`git merge-file` on `fork_blob`); propose changes upstream (converting links
back); tag filters (need file contents); offer to fork or mirror the notes dangling links name;
warn when a local copy is ahead of its origin; mirror attachments; update mirrors at each check
if wanted.

## Proposed for the publictext spec

Not authoritative there yet.

- Reserve `fork_remote`, `fork_repo`, `fork_path`, `fork_branch`, `fork_commit`, `fork_blob`,
  `fork_date`, `fork_seen` (§5) as authored provenance. (Mirrors are §5.5, ADR-0017.)
- `follows` entries gain `branch` and `include`: the simplest piece of the filter grammar
  (§13 q1).
- A v0 canonical repo URL (§13 q2).
