---
bookmark: https://github.com/publictxt/publictext
tags: [publictxt, project, docs]
---
# PublicTxt overview

PublicTxt is an experiment in using Git repositories as an interoperability layer and online storage for public and community knowledge. By using free Git hosting and plain-text Markdown, individuals and communities can share, curate and collaborate on content without a centralised platform or paid infrastructure.

Desktop and web tools sync between local working copies and one or more Git repositories, **selectively aggregate content from other people's repositories**, and publish to the web with static site generation — all on plain Markdown that outlives the tooling.

## What it is, for now

The differentiator is **selective aggregation across many Git-hosted repositories**: subscribing to other people's PublicTxt repositories and pulling chosen subsets (by tag, path or filter) into a local aggregate view. Markdown in Git, published as a static site, is the substrate; aggregation is the value.

v1 success looks like:

> I can keep my notes and writing in a Git repository, publish them to the web for free via GitHub Pages or similar, and pull selected pages from other people's PublicTxt repos into a local aggregate view I can browse and search.

Community features, browser extensions, federation and graph-database backends may come later. They aren't v1. See the [roadmap](Roadmap.md).

## Core ideas

- **Plain text first** — Markdown with standard `[text](page.md)` links, readable by any static-site host with no conversion. Tags are front matter `tags:` or inline `#hashtags`.
- **Zero-cost storage and hosting** — free Git hosting for storage, distribution and static sites.
- **Static website generation** — [PublicTxt-Pages](PublicTxt-Pages.md): push and you're live, with full-text search and facets. See [Publishing](Publishing.md).
- **Selective aggregation** — pull chosen content from other repositories into a local aggregate (not built yet).
- **Obsidian compatible** — a PublicTxt repository is an Obsidian vault. [Obsidian PublicTxt Facets](Obsidian-PublicTxt-Facets.md) brings the site's browsing into the editor.

## Use cases

- Personal knowledge management, with version control and free hosting
- Distributed blogging others can subscribe to and aggregate
- Research and learning: pull peers' content into a local view
- Lightweight community wikis, using Git's own collaboration (pull requests, branches, forks)

## Licence and contributing

GPLv3 — see [LICENSE](https://github.com/publictxt/publictext/blob/main/LICENSE). Contributing: [CONTRIBUTING.md](https://github.com/publictxt/publictext/blob/main/CONTRIBUTING.md).
