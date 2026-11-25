---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/naming.md
mirror_branch: master
mirror_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
mirror_blob: 09e71df643e7b4f190cd6b1b49fe712896a26130
mirror_date: 2026-11-25T22:45:40+02:00
---
# Naming

## Frozen at first release

| Thing | Name |
|---|---|
| Repo | `publictxt/obsidian-publictxt-remotes` |
| Plugin `id` / name | `publictxt-remotes` / PublicTxt Remotes |
| View types | `publictxt-check`, `publictxt-upstream` |
| Command ids | `check-sources`, `open-check-view`, `mirror-file`, `stop-mirroring`, `fork-mirror`, `fork-file`, `compare-fork`, `convert-links`, `match-links`, `update-page-lists` |

## Vocabulary

| Word | Means |
|---|---|
| source | A followed repo, hosted or local: a follow note in `follows/` |
| follow | Add a repo as a source. Only repos are followed; pages are mirrored or forked |
| remote name | A source's name: its follow note's file name. Copies record it (`*_remote`) |
| source key | `Source.url`: canonical URL, or `local:<name>` |
| remote | The technical word for a source (plugin name, `*_remote`). Never a folder |
| include | Folder prefixes narrowing "what's new" |
| local source | A source on this computer: `local: true` in its follow note |
| local copy | A hosted source's working copy on this computer |
| snapshot / make a repo | Commit a local folder's changes / `git init` it with a `.gitignore`, then snapshot |
| clone | A source's bare partial clone under `clonesFolder`. Never a page copy |
| mirror | Upstream's copy of a page under the community folder, with `mirror_*` keys; overwritten by updates. Also the action |
| fork | The user's own note made from a source file, with `fork_*` keys |
| copy | A fork or mirror; for forks, one of the notes from one upstream file |
| latest | The copy with the newest `fork_date`: the one compared |
| settled | A copy needing nothing for an upstream blob: its `fork_blob` or `fork_seen` is it |
| compare | A fork beside the Upstream view |
| community folder | Holds each source's **mirror folder** (`communityFolder`; the source's `folder`, else its remote name) |
| fork folder | Default fork destination (`forkFolder`) |
| mirror list / fork list | The "Mirrored pages" / "Forked pages" sections in a follow note |
| convert links | Write a fork's links in the vault's form, paths kept |
| match links | Point broken links at the one note of that name |
| check | One pass over the sources: what's new, mirrors, forks |
| head / blob | The tracked branch's latest commit / a file's git blob SHA |

## Code layout

Each feature file in [features/](features/) names its modules. Tests are in
`tests/<module>.test.ts`.

| Folder | Holds |
|---|---|
| `src/sources/` | `data.json` schema, follow notes, canonical URL, provenance keys, scanning notes |
| `src/remote/` | `RemoteHost`, `GitHost`, local folders, snapshot |
| `src/check/` | A check and its vault run; file tree; ticks |
| `src/copy/` | What forks and mirrors share. Imports neither; `fork/` may import `mirror/`, never the reverse |
| `src/mirror/` | Mirroring, update, stop |
| `src/fork/` | Fork destination and writing, fork update, lists, fork links |
| `src/snapshot/` | Snapshot and make-a-repo from the vault |
| `src/ui/` | Views, modals, tree rows |
| `src/util/` | Small helpers |
