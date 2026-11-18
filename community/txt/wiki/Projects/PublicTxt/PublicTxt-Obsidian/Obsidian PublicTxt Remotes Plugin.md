---
category:
  - CompSci
  - Project
  - Tech
tags:
  - dev
  - obsidian
  - plugin
  - publictxt
  - project
  - dev/planned
web: https://github.com/publictxt/obsidian-publictxt
bookmark: https://github.com/publictxt/obsidian-publictxt
created: 2026-11-02
mirror_remote: txt
mirror_repo: https://github.com/jaysen/txt
mirror_path: wiki/Projects/PublicTxt/PublicTxt-Obsidian/Obsidian PublicTxt Remotes Plugin.md
mirror_branch: master
mirror_commit: 7b95c242b19930703ed48bba02beba6bb0261c61
mirror_blob: 777c7ec5e0246d6df34039e1e82a5a117de5ddbd
mirror_date: 2026-11-17T21:15:12+02:00
---
## overview
- Adds collaborative features - to 'connect' [Obsidian.md](community/txt/wiki/Computer-Science/Software/Obsidian/Obsidian.md.md) wikis to other [PublicTxt Wikis](PublicTxt%20Wikis)
- Should work well with [Obsidian PublicTxt Facets Plugin](community/txt/wiki/Projects/PublicTxt/PublicTxt-Obsidian/Obsidian%20PublicTxt%20Facets%20Plugin.md) -- which replaced the [Obsidian Search Facets Plugin](community/txt/wiki/Projects/Obsidian%20Plugins/Obsidian%20Search%20Facets%20Plugin.md)

Follow other people's PublicTxt repos, see what changed since you last looked, and fork the
pages you want into your vault. Each fork records where it came from in its front matter.

> **Status: early development.** Nothing is released. The data model and the git layer are
> built and tested. The settings tab, check view and fork command are in progress.

## Requirements

- Obsidian desktop. The plugin uses the git CLI, so it doesn't run on mobile.
- [git](https://git-scm.com/) installed. If Obsidian can't find it on your `PATH` (common on
  macOS when Obsidian is started from Finder), set **Git executable** in the plugin settings.

## How it works

- **Sources** are remote repos you follow, optionally narrowed to some folders.
- Each source is kept as a bare partial clone in your vault's `remotes/` folder. File contents
  are fetched only when needed. Consider adding `remotes/` to your vault's `.gitignore` and
  excluding it from Obsidian Sync.
- A **fork** is a note copied from a source, with `fork_repo`, `fork_path`, `fork_blob` and
  related keys in its front matter. Those keys are how the plugin tells when the original
  has changed.

## Settings

| Setting            | Default    | Meaning                   |
| ------------------ | ---------- | ------------------------- |
| **Fork folder**    | vault root | Where forked notes go     |
| **Remotes folder** | `remotes`  | Where the git clones live |
| **Git executable** | `git`      | Path to git               |
