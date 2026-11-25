---
bookmark: https://github.com/publictxt/obsidian-publictxt-remotes
tags: [publictxt, obsidian, plugin, docs]
---
# Obsidian PublicTxt Remotes

An Obsidian plugin that follows other people's PublicTxt repos, shows what changed since you last looked, mirrors the pages you want at hand, and forks the ones you want to make your own. It is the pulling half of PublicTxt's aggregate view; [Obsidian PublicTxt Facets](../publictxt-facets/Obsidian-PublicTxt-Facets.md) is the browsing half.

- **Follow notes**: one note per repo in `follows/`, named for the remote, with its `url`, `branch` and `include` in front matter and why you follow it in the body (spec §3.2).
- **Check**: per repo, what's new since you last looked, your mirrors that are behind, and your forks whose original changed. By git blob SHA, so it's the same on every host.
- **Mirror**: keep upstream's copy of a page in `community/<remote>/`, at the path it has there, with `mirror_*` provenance (spec §5.5). Obsidian searches it and shows its backlinks; an update overwrites it. Each follow note can list the pages mirrored from it.
- **Fork**: make a page your own, in a folder you choose (a mirrored page moves there), with `fork_*` provenance in its front matter, including the remote's name as `fork_remote` (spec §5.2). Each follow note can list the pages forked from it.
- **Local sources**: a git repo on your computer, such as another vault, can be a source too. Its follow note holds no path, so it can be published; the folder stays in the plugin's settings. **Snapshot** commits what changed in it; the plugin can make a plain folder a repo.
- **Local copies**: a repo you follow by URL that is also cloned on your computer stays one source. Set its folder as the source's local copy and the plugin fetches from there, so checks see commits you haven't pushed yet.

Desktop only, over the git CLI: any host, your own SSH keys and credential helpers. Clones live in the vault's hidden `.clones/` folder.

Decisions: [ADR-0011](docs/adr/0011-obsidian-plugin-follows-with-git-cli.md), [ADR-0012](docs/adr/0012-fork-provenance-in-front-matter.md), [ADR-0014](docs/adr/0014-follows-are-notes.md), [ADR-0015](docs/adr/0015-obsidian-remotes-local-sources-desktop-only.md), [ADR-0017](docs/adr/0017-mirrors-in-community.md), [ADR-0018](docs/adr/0018-local-copy-of-a-hosted-follow.md).

## Follow, mirror, fork

Reading someone's notes and owning a copy stay apart:

1. **Follow** a repo: a note in `follows/`. Whole repos, narrowed locally by `include` folders (ADR-0005).
2. **Mirror** the pages worth having at hand: upstream's copies under `community/<follow>/`, overwritten by updates, not for editing. A mirror's links go by full path into its mirror folder, so they never land on your same-named note.
3. **Fork** the ones to make your own: your note, wherever you put it. Its links are pointed at your copies when you accept the offer; links to pages you haven't copied go by name. Upstream changes arrive as a compare (overwrite, write alongside, or keep yours), not a merge. Forking a mirror moves it.

Notes carry their own facts: lists of mirrors and forks are derived by scanning their keys, never stored. Settings, a cache and per-computer paths live in the plugin's `data.json`, never in a note.

## Documentation

In the [repository](https://github.com/publictxt/obsidian-publictxt-remotes)'s `docs/`, mirrored here under `community/publictxt-remotes/`. Nothing is released; names and ids can change until the first release.

| File | Holds |
|---|---|
| [intent.md](https://github.com/publictxt/obsidian-publictxt-remotes/blob/master/docs/intent.md) | Why, the decisions, what's open |
| [naming.md](https://github.com/publictxt/obsidian-publictxt-remotes/blob/master/docs/naming.md) | Vocabulary, frozen ids, code layout |
| [features/](https://github.com/publictxt/obsidian-publictxt-remotes/tree/master/docs/features) | One file per feature: sources, check, git, local, mirror, fork, fork update, lists, convert links |
| [history.md](https://github.com/publictxt/obsidian-publictxt-remotes/blob/master/docs/history.md) | Past names and dropped designs: was `obsidian-publictxt`; clones were in `.remotes/`, mirrors and forks in `remotes/<source>/`; `watch` became mirrors |

## Open questions

- Removing a follow leaves its clone: delete it (asking), add a clean-up command, or leave it.
- `fork_remote` as a link (`[[follows/name]]`): clickable and rename-safe if Obsidian updates links in properties.
- Obsidian Sync carries `data.json`, so a local folder set on one computer shows on another where it's likely missing.
- A setting to always create the source's folders on fork.
- A hosted source with a local copy: when history has been rewritten, check which is ahead.

## Later

Merge a fork (`git merge-file` on `fork_blob`); propose changes upstream, converting links back; tag filters; offer to fork or mirror the pages a dangling link names; warn when a local copy is ahead of its origin; mirror attachments; update mirrors at each check; per-source link style.

## Links

- [Repository](https://github.com/publictxt/obsidian-publictxt-remotes)
- Earlier notes: [Obsidian PublicTxt Remotes Plugin](publictxt-remotes/Obsidian%20PublicTxt%20Remotes%20Plugin.md), [PublicTxt Remotes Local Copies](PublicTxt%20Remotes%20Local%20Copies.md)
