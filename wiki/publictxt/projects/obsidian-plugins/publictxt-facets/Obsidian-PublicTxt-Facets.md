---
bookmark: https://github.com/publictxt/obsidian-publictxt-facets
tags: [publictxt, obsidian, plugin, docs]
---
# Obsidian PublicTxt Facets

An Obsidian plugin that brings [PublicTxt-Pages](../../publictxt-pages/PublicTxt-Pages.md)' browse lists into Obsidian. Works in any vault: nothing requires PublicTxt conventions.

- **Chips** for each collection, category and tag: click to include, ✕ to exclude.
- **All or any** when a facet has two or more values included.
- **Live counts**: each chip shows how many pages it would leave.
- **Rating** and **title** filters, and sorts by date, title or rating.
- **Cards** with title, collections, dates, summary, tags and bookmark links; a compact view.

It reads the same conventions as the site (top-level folders as collections, front matter tags and inline hashtags as one tag set), so while you write, it shows roughly what the site will list. It's meant to be the browsing part of PublicTxt's aggregate view, though it doesn't aggregate anything itself.

It is not a replacement for core search (no query syntax, and no text search yet), never changes the vault, and shares the site's concepts, not its code or styling. Its niche among Obsidian plugins is combinable, counted, read-only filtering with cards; Facet Folders, the nearest, is a tree navigator without combined filters or counts.

## Documentation

In the [repository](https://github.com/publictxt/obsidian-publictxt-facets)'s `docs/`, mirrored here under `community/publictxt-facets/`.

| File | Holds |
|---|---|
| [intent.md](https://github.com/publictxt/obsidian-publictxt-facets/blob/master/docs/intent.md) | Why, who for, decisions, design tensions with the site, open questions |
| [spec.md](https://github.com/publictxt/obsidian-publictxt-facets/blob/master/docs/spec.md) | Page record, filtering and counts, sort, view, commands, settings |
| [naming.md](https://github.com/publictxt/obsidian-publictxt-facets/blob/master/docs/naming.md) | Plugin id, commands, code identifiers, vocabulary; renamed from Txt Facets, which replaced Obsidian Search Facets |

## Open questions

- **Text search: Obsidian's or its own?** Obsidian's core search has no public API, so "integrate" likely means a command that hands the current facets to core search as a query. Search inside the view means its own engine (MiniSearch, or substring matching first).
- **Does the title filter become the search box** when text search arrives?
- **"Show pages like this one"** matches all of the page's tags, which often finds little. Any would find more.

## Planned

- **Community filters**: source, source rating and copy (forked, mirrored), as on the site (spec §5.4), so pages from [Remotes](publictxt-remotes/Obsidian-PublicTxt-Remotes.md) can be told apart from your own.

## Links

- [Repository](https://github.com/publictxt/obsidian-publictxt-facets)
- Earlier notes: [Obsidian PublicTxt Facets Plugin](publictxt-facets/Obsidian%20PublicTxt%20Facets%20Plugin.md)
