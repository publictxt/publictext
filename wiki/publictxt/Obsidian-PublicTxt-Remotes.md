---
bookmark: https://github.com/publictxt/obsidian-publictxt-remotes
tags: [publictxt, obsidian, plugin, docs]
---
# Obsidian PublicTxt Remotes

An Obsidian plugin that follows other people's PublicTxt repos, shows what changed since you last looked, and forks the pages you want into your vault. It is the pulling half of PublicTxt's aggregate view; [Obsidian PublicTxt Facets](Obsidian-PublicTxt-Facets.md) is the browsing half.

- **Follow notes**: one note per repo in `follows/`, named for the remote, with its `url`, `branch` and `include` in front matter and why you follow it in the body (spec §3.2).
- **Check**: per repo, what's new since you last looked, files you watch, and your forks whose original changed. By git blob SHA, so it's the same on every host.
- **Fork**: copy a page into a folder you choose, with `fork_*` provenance in its front matter, including the remote's name as `fork_remote` (spec §5.2). Each follow note can list the pages forked from it.
- **Local sources**: a git repo on your computer, such as another vault, can be a source too. Its follow note holds no path, so it can be published; the folder stays in the plugin's settings. **Snapshot** commits what changed in it; the plugin can make a plain folder a repo.

Desktop only, over the git CLI: any host, your own SSH keys and credential helpers. Clones live in the vault's hidden `.remotes/` folder.

Decisions: [ADR-0011](docs/adr/0011-obsidian-plugin-follows-with-git-cli.md), [ADR-0012](docs/adr/0012-fork-provenance-in-front-matter.md), [ADR-0014](docs/adr/0014-follows-are-notes.md), [ADR-0015](docs/adr/0015-obsidian-remotes-local-sources-desktop-only.md).

- [Repository](https://github.com/publictxt/obsidian-publictxt-remotes)
