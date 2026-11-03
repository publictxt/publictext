# PublicTxt

PublicTxt is an experiment in using Git repositories as an interoperability layer and online storage for public and community knowledge. Using free Git hosting and plain-text Markdown, individuals and communities can share, curate and collaborate on content without relying on centralized platforms or paid infrastructure.

At its core is a **format specification**: plaintext conventions that any tool can read and write. Tools built on it sync your repository, follow and aggregate other people's repositories, and publish everything to the web as a static site.

The experiment is in early stages.

## What PublicTxt is, for now

The differentiator is **selective aggregation across many Git-hosted repositories**: following other people's PublicTxt repos and pulling chosen pages into a local view. Plain-text Markdown in Git, published as a static site, is the *substrate*; aggregation is the *value*. Everything else supports that, or is deferred.

v1 success looks like:

> "I can keep my notes and writing in a Git repository, publish them to the web for free via gh-pages or similar, and pull selected pages from other people's PublicTxt repos into a local aggregate view I can browse and search."

## How it works

- **Plain text.** Markdown with YAML front matter and inline `#tags`, compatible with Obsidian Properties.
- **Git is the storage.** A repository holds only what its author wrote. Indexes, tag pages and search are rebuilt from Git and never committed.
- **One author per repository.** A repository is an identity. Multi-author views come from following and aggregating repositories.
- **Follow, then filter.** You follow whole repositories and filter when reading.
- **Fork with provenance.** A page copied from another repo records where it came from in its own front matter, so you can see when the source changes.
- **Tags as written.** Tags stay exactly as the author wrote them. Each reader reconciles vocabulary with their own alias file.
- **Bookmarks on any page.** A page with a `bookmark: <url>` attribute is a bookmark, whatever its type.
- **Zero-cost publishing.** Repositories publish as static sites on free Git hosting such as GitHub Pages or GitLab Pages.
- **Use the editor you have.** Write in Obsidian or any Markdown editor. PublicTxt tooling focuses on following, aggregating and publishing.

## Status

| Part | State |
|---|---|
| [Format specification](wiki/publictxt/docs/spec/publictxt-spec.md) v0.1 and [conformance fixtures](wiki/publictxt/docs/spec/fixtures/) | Draft |
| [Architecture decisions](wiki/publictxt/docs/adr/) | Recorded; several still proposed |
| [PublicTxt-Pages](#publictxt-pages): static site | Working: live at [jaysen.co.za](https://jaysen.co.za) |
| [Obsidian PublicTxt Facets](#obsidian-publictxt-facets): browse a vault by facets | v1.0 released |
| [Obsidian PublicTxt](#obsidian-publictxt): follow repos, fork pages | In progress |
| [WikiTool](#wikitool) | In progress |
| [PublicTxt.net](#publictxtnet): .NET implementation | In progress |
| [CuratedCommons](#curatedcommons) | Specification stage |
| `ptx` CLI: validate, publish, follow, aggregate | Planned |
| Desktop and web apps | Deferred ([ADR-0008](wiki/publictxt/docs/adr/0008-obsidian-is-the-editor.md)) |

## Components

### PublicTxt-Pages

[publictxt/publictxt-pages](https://github.com/publictxt/publictxt-pages): zero-backend static site for a PublicTxt repository, or any Obsidian or Markdown wiki.

- Builds with Hugo; deploys to GitHub Pages or any static host.
- Works on plain Obsidian vaults that use relative Markdown links, with no conversion step.
- Full-text search and facets (tags, tag combinations, type) via [Pagefind](https://pagefind.app/).
- Single repo in v1.

### Obsidian PublicTxt Facets

[publictxt/obsidian-publictxt-facets](https://github.com/publictxt/obsidian-publictxt-facets): the same faceted browsing inside Obsidian. Offline by design.

- Filter notes by combinations of tags; the tag list narrows to tags that co-occur with the selection.

### Obsidian PublicTxt

[publictxt/obsidian-publictxt](https://github.com/publictxt/obsidian-publictxt): the pulling half of v1.

- Follow whole repos; narrow what's shown by folder (`include`) and watch single files.
- See what changed since you last looked, by git blob SHA.
- Fork a page into your vault, with `fork_*` provenance in its front matter.
- Desktop-only, over the git CLI: any host, your own SSH keys. Mobile is a later phase.
- Decisions: [ADR-0011](wiki/publictxt/docs/adr/0011-obsidian-plugin-follows-with-git-cli.md), [ADR-0012](wiki/publictxt/docs/adr/0012-fork-provenance-in-front-matter.md).

### WikiTool

[jaysen/WikiTool](https://github.com/jaysen/WikiTool): multiplatform .NET tool for handling multiple wikis.

- Converts between wiki formats and syntaxes, including Obsidian ↔ md-wiki.
- Copies between wikis by search, tag expressions (`tag1 + tag2 - tag3`) and attributes.

### PublicTxt.net

[publictxt/publictxt.net](https://github.com/publictxt/publictxt.net): .NET implementation. Core libraries, feature services, Git and database infrastructure.

### CuratedCommons

[publictxt/curated-commons](https://github.com/publictxt/curated-commons): social bookmarking and curation on PublicTxt repos. Commentary lives in plain-text repos that are public, forkable and followable. See [CuratedCommons](wiki/publictxt/CuratedCommons.md).

## Repository layout

Two layouts exist today:

- **Working layout**, used by this repo and PublicTxt-Pages: `wiki/`, `blog/`, `posts/`, `notes/`, `bookmarks/sites/<domain>/`, `media/`. See [Repository structure](wiki/publictxt/Repository-Structure.md).
- **Spec default layout** (spec §4, draft): `wiki/`, `blog/<yyyy>/<mm>/`, `notes/`, `metaweb/<domain>/…`, `media/`, plus a `publictxt.yaml` manifest. Folders can be remapped in the manifest, so an existing vault can adopt PublicTxt without restructuring.

Reconciling the two is open. Until then, the working layout is what tools build against.

A minimal manifest (spec §3):

```yaml
publictxt: "0.1"
author:
  name: Your Name
follows:
  - url: https://github.com/example/notes
```

Front matter on a wiki page that is also a bookmark:

```yaml
---
title: Pro Git
date: 2026-10-13T00:36:00+02:00
tags: [git, books]
bookmark: https://git-scm.com/book/en/v2
---
```

## Use cases

- **Personal knowledge management:** your own Markdown knowledge base with version control and free hosting.
- **Distributed blogging:** posts that others can follow and aggregate.
- **Research and learning:** pull selected pages from peers' repos into a local view for study and synthesis.
- **Social bookmarking (CuratedCommons):** share and annotate web resources; aggregate other people's bookmarks and tags.
- **Community wikis (lightweight):** small groups using Git's own collaboration: pull requests, branches, forks.

## Later

Deferred until the v1 aggregation-and-publish story works and is used. Each has a trigger in [IDEAS.md](wiki/publictxt/docs/IDEAS.md); the [Roadmap](wiki/publictxt/Roadmap.md) has the order.

- **Visibility:** public, semi-public and private content. Private content needs private repos; how they fit into following is undecided.
- **Community:** profiles, groups, discussion and moderation (`community/`).
- **Desktop and web apps** that hide Git, with a local cache for fast search.
- **MetaWeb:** a browser extension showing Git-stored notes on web pages; W3C Web Annotation export.
- **Federation:** ActivityPub; Git commits as public records.
- **Richer data:** RDF semantic edges, weighted links, a TerminusDB graph backend, consensus and reputation.

## This repository

A working PublicTxt repository, published as [jaysen.co.za](https://jaysen.co.za) (see [Publishing](wiki/publictxt/Publishing.md)). It also holds the project's own docs:

- `wiki/publictxt/`: project documentation
- `wiki/publictxt/docs/spec/`: format specification and conformance fixtures
- `wiki/publictxt/docs/IDEAS.md`: parked ideas, each with a trigger for revisiting it
- `wiki/publictxt/docs/adr/`: architecture decision records
- `wiki/publictxt/docs/history/`: earlier versions of this README

Suggest changes through issues, pull requests or by forking.

## License

GPLv3: https://www.gnu.org/licenses/gpl-3.0.en.html. See [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
