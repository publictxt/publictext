---
tags: [publictxt, docs, roadmap]
---
# Roadmap

## Status

| Component | Status |
|---|---|
| [Obsidian PublicTxt Facets](projects/obsidian-plugins/publictxt-facets/Obsidian-PublicTxt-Facets.md) | v1.0 released |
| [Obsidian PublicTxt Remotes](projects/obsidian-plugins/publictxt-remotes/Obsidian-PublicTxt-Remotes.md) | In progress — follow repos, mirror and fork pages (desktop) |
| [PublicTxt-Pages](projects/publictxt-pages/PublicTxt-Pages.md) | Working — this site, [publictxt.github.io/publictext](https://publictxt.github.io/publictext/) |
| Format specification | Draft; the [working layout](Repository-Structure.md) follows PublicTxt-Pages |
| [WikiTool](projects/WikiTool.md) | In progress |
| [PublicTxt.net](projects/PublicTxt.net.md) | In progress |
| [CuratedCommons](projects/CuratedCommons.md) | Specification stage |

## v1

1. Keep notes and writing in a Git repository — done
2. Publish to the web for free — done ([Publishing](projects/publictxt-pages/Publishing.md))
3. Pull selected pages from other people's PublicTxt repositories into a local aggregate view to browse and search — in progress: Obsidian PublicTxt Remotes follows, mirrors and forks; Obsidian PublicTxt Facets browses

## Next

- **Obsidian PublicTxt Remotes**: first release. Then merging forks and proposing changes upstream ([Later](projects/obsidian-plugins/publictxt-remotes/Obsidian-PublicTxt-Remotes.md#later)).
- **Obsidian PublicTxt Facets**: source, source rating and copy filters, so mirrored and forked pages can be told apart while browsing.
- **PublicTxt-Pages**: `[[wikilinks]]` at sync (on hold), folder pages by any name, tag feeds ([Planned](projects/publictxt-pages/PublicTxt-Pages.md#planned)).

## Later

- **Desktop and web apps** that hide Git, with a local cache for fast search
- **Community**: discussion, profiles, groups, moderation; public / semi-public / private content
- **MetaWeb**: a browser extension showing Git-stored notes about web pages; W3C Web Annotation export
- **Federation**: ActivityPub; Git commits as public records
- **Richer data**: RDF semantic edges, [weighted links and attributes](docs/ideas/weighted-links-and-attributes.md), a TerminusDB graph backend, reputation and peer review

Deferred until the v1 aggregation-and-publish story works and is used. Triggers for each are in [IDEAS.md](docs/IDEAS.md).
