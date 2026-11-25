---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/README.md
mirror_branch: master
mirror_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
mirror_blob: 5a7ae3ef0c50aa9a2c54b507e7551e0814e4364d
mirror_date: 2026-11-25T22:45:39+02:00
---
# Docs

Read only what the task needs.

| File | Holds |
|---|---|
| [intent.md](intent.md) | Why the plugin exists, the decisions that shape it, what's open |
| [naming.md](naming.md) | Vocabulary, frozen ids, where the code lives |
| [features/](features/) | What each feature does: the reference. One file per feature |

Features:

| File | Covers |
|---|---|
| [sources.md](features/sources.md) | `data.json`, follow notes, canonical URL, settings tab, state |
| [check.md](features/check.md) | A check, and the check view |
| [git.md](features/git.md) | `RemoteHost` over the git CLI: clones, commands, errors |
| [local.md](features/local.md) | Local sources, local copies, making a repo, snapshot |
| [mirror.md](features/mirror.md) | Mirror provenance; mirror, update, stop |
| [fork.md](features/fork.md) | Fork provenance; the fork modal |
| [fork-update.md](features/fork-update.md) | Compare, overwrite, write alongside, keep, detach |
| [lists.md](features/lists.md) | Mirror and fork lists in follow notes |
| [fork-links.md](features/fork-links.md) | A fork's links: converting their form, matching broken ones |

Each feature file ends with its code. Nothing is released yet: everything, ids included, can
still change until the first release.
