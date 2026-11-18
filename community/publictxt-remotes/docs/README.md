---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/README.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: fdc6c0c3f836cd7749c563016531625ec5c4acd7
mirror_date: 2026-11-17T21:07:37+02:00
---
# Docs

Read only what the task needs.

| File | Holds |
|---|---|
| [intent.md](community/publictxt-remotes/docs/intent.md) | Why the plugin exists, the decisions that shape it, what's open |
| [naming.md](community/publictxt-remotes/docs/naming.md) | Vocabulary, frozen ids, where the code lives |
| [features/](features/) | What each feature does: the reference. One file per feature |
| [history.md](community/publictxt-remotes/docs/history.md) | Past renames and dropped designs. Rarely needed |

Features:

| File | Covers |
|---|---|
| [sources.md](community/publictxt-remotes/docs/features/sources.md) | `data.json`, follow notes, canonical URL, settings tab, state |
| [check.md](community/publictxt-remotes/docs/features/check.md) | A check, and the check view |
| [git.md](community/publictxt-remotes/docs/features/git.md) | `RemoteHost` over the git CLI: clones, commands, errors |
| [local.md](community/publictxt-remotes/docs/features/local.md) | Local sources, local copies, making a repo, snapshot |
| [mirror.md](community/publictxt-remotes/docs/features/mirror.md) | Mirror provenance; mirror, update, stop |
| [fork.md](community/publictxt-remotes/docs/features/fork.md) | Fork provenance; the fork modal |
| [fork-update.md](community/publictxt-remotes/docs/features/fork-update.md) | Compare, overwrite, write alongside, keep, detach |
| [lists.md](community/publictxt-remotes/docs/features/lists.md) | Mirror and fork lists in follow notes |
| [convert-links.md](community/publictxt-remotes/docs/features/convert-links.md) | Pointing a copy's links at the vault's copies |

Each feature file ends with its code. Nothing is released yet: everything, ids included, can
still change until the first release.
