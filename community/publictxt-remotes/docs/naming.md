---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/naming.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 181e961a9cafd0081c7fc329815e428bc6f13d64
mirror_date: 2026-11-17T21:07:37+02:00
---
# Naming

## Frozen at first release

| Thing | Name |
|---|---|
| Repo | `publictxt/obsidian-publictxt-remotes` |
| Plugin `id` / name | `publictxt-remotes` / PublicTxt Remotes |
| View types | `publictxt-check`, `publictxt-upstream` |
| Command ids | `check-sources`, `open-check-view`, `mirror-file`, `stop-mirroring`, `fork-mirror`, `fork-file`, `compare-fork`, `convert-links`, `convert-links-all`, `update-page-lists` |

## Vocabulary

| Word | Means |
|---|---|
| source | A followed repo, hosted or local: a follow note in `follows/` |
| follow | Add a repo as a source. Only repos are followed; pages are mirrored or forked |
| remote name | A source's name: its follow note's file name. Copies record it (`*_remote`) |
| source key | `Source.url`: canonical URL, or `local:<name>` |
| remote | The technical word for a source (plugin name, `*_remote`). Never a folder |
| include | Folder prefixes narrowing "what's new" |
| local source | A source on this computer: `local: true`, folder in `localPaths` |
| local copy | A hosted source's working copy here, fetched from when **Local** is chosen (`checkFrom`). Still one source |
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
| links to fix / to check | Follow note sections: links converting left / made a call on |
| link by name | A link without a path (`[[b]]`, `[b](b.md)`) |
| check | One pass over the sources: what's new, mirrors, forks |
| head / blob | The tracked branch's latest commit / a file's git blob SHA |

## Code layout

Pure modules (no Obsidian, tested under `node --test`) and their vault/UI sides. Each feature
file in [features/](features/) names its modules.

| Folder | Holds |
|---|---|
| `src/sources/` | `data.json` schema, follow notes, canonical URL, provenance keys, scanning notes |
| `src/remote/` | `RemoteHost`, `GitHost`, local folders, snapshot; `git-process.ts` is the only Node module |
| `src/check/` | A check and its vault run; file tree; ticks |
| `src/mirror/` | Mirroring, update, stop |
| `src/fork/` | Fork destination and writing, fork update, lists, convert links |
| `src/snapshot/` | Snapshot and make-a-repo from the vault |
| `src/ui/` | Views, modals, tree rows |
| `src/util/` | Small helpers |

Conventions: a pure `x.ts` has its vault side in `x-run.ts` or `run.ts`; tests in
`tests/<module>.test.ts`.
