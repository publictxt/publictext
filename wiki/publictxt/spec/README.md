# PublicTxt

## Overview

PublicTxt is an experiment in using Git repositories as an interoperability layer and online storage for public and community knowledge. Using free Git hosting and plain text Markdown, individuals and communities can share, curate and collaborate on content without relying on centralized platforms or paid infrastructure.

At its core is a **format specification**: plaintext conventions that any tool can read and write. Tools built on it sync your repository, follow and aggregate other people's repositories, and publish everything to the web as a static site.

The experiment is in early stages.

## Status

| Part | State |
|---|---|
| [Format specification](spec/publictxt-spec.md) v0.1 | Draft |
| [Conformance fixtures](spec/fixtures/) | Draft |
| [Architecture decisions](docs/adr/) | Recorded; several still proposed |
| publictxt-pages: static site with Hugo and Pagefind | Scaffold |
| `ptx` CLI: validate, publish, follow, aggregate | Planned |
| .NET reference implementation | Planned |
| Desktop and web apps | Deferred ([ADR-0008](docs/adr/0008-obsidian-is-the-editor.md), proposed) |

## How it works

- **Plain text.** Markdown with YAML front matter and inline `#tags`, compatible with Obsidian.
- **Git is the storage.** A repository holds only what its author wrote. Indexes, tag pages and search are rebuilt from Git and never committed.
- **One author per repository.** A repository is an identity. Multi-author views come from following and aggregating repositories.
- **Follow, then filter.** You follow whole repositories, listed in your `publictxt.yaml`, and filter by tag when reading.
- **Tags as written.** Tags stay exactly as the author wrote them. Each reader reconciles vocabulary with their own alias file.
- **Bookmarks on any page.** A page with a `bookmark: <url>` attribute is a bookmark, whatever its type. Bookmarks of the same resource are matched across repositories by `resource_id`.
- **Zero-cost publishing.** Repositories publish as static sites on free Git hosting such as GitHub Pages or GitLab Pages.
- **Use the editor you have.** Write in Obsidian or any Markdown editor. PublicTxt tooling focuses on following, aggregating and publishing.

## Use cases

- **Personal and community knowledge management:** your own Markdown knowledge base with version control.
- **Community wikis:** knowledge bases built from many contributors' repositories.
- **Social bookmarking (CuratedCommons):** share and annotate web resources, and aggregate other people's bookmarks and tags.
- **Distributed blogging:** blog posts that others can follow and aggregate.
- **Research collaboration:** share research notes and findings.
- **Learning communities:** shared knowledge bases around a topic or field.

## Repository layout

The default layout is below (spec §4). Folders can be remapped in `publictxt.yaml`, so an existing Obsidian vault can adopt PublicTxt without restructuring.

```
publictxt.yaml        # manifest: spec version, author, folder→type map, follows
wiki/                 # wiki pages
blog/<yyyy>/<mm>/     # blog posts, e.g. 20260920-1-title.md
notes/                # notes
metaweb/<domain>/…    # one file per bookmarked web resource
media/                # attachments
```

A minimal manifest:

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

## Not yet in the spec

- **Visibility:** public, semi-public and private content. Private content needs private repositories; how they fit into following and aggregation is undecided.
- **Community:** profiles, groups and discussion (`community/`).

## This repository

- A working example of the format. It also holds the wiki and other content about PublicTxt itself.
- `spec/`: the format specification and conformance fixtures.
- `docs/adr/`: architecture decision records.
- `IDEAS.md`: parked ideas and when to revisit them, including a browser extension for MetaWeb notes on web pages, ActivityPub, immutable public records, RDF semantic edges, a TerminusDB backend, and consensus and reputation mechanisms.
- Suggest changes through issues, pull requests or by forking.

## License

GPLv3: https://www.gnu.org/licenses/gpl-3.0.en.html. See [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
