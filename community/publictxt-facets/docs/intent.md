---
mirror_remote: publictxt-facets
mirror_repo: https://github.com/publictxt/obsidian-publictxt-facets
mirror_path: docs/intent.md
mirror_branch: master
mirror_commit: 11eb3c44b065e5040c2708e2da5ab5c6d38bd2fe
mirror_blob: 910a9a64a56019ff4b2133f7b1c973f2bd8df8b5
mirror_date: 2026-11-25T22:45:16+02:00
---
# PublicTxt Facets — Intent

> Carries the archive's intent forward (`obsidian-search-facets-archived/docs/intent.md`),
> updated for [PublicTxt-Pages](https://github.com/publictxt/publictxt-pages) (formerly txt-hugo)
> and the decisions below. Names are defined in `naming.md`.

An Obsidian plugin for browsing a vault by **facets**: chip facets with live counts (collection,
category, tags, rating), a sort menu, and a list of page cards. It shares the core concepts and
features of PublicTxt-Pages' browse lists and search page, so it suits a PublicTxt repository,
but it is a general facet browser and works in any vault.

It starts as **facets only, with no full-text search**. Text search may come later (see
*Open questions*).

## Why

The PublicTxt-Pages site is where people *read* a PublicTxt repository. Obsidian is where its
author *writes* it. The two offer different ways to find things:

- **The site** browses by facets. Chips include or exclude values, several values match all or
  any, and counts reflect the current results ("everything tagged `#brain` and `#philosophy`,
  but not `#journal`"). Results are cards, sortable by date, title or rating.
- **Obsidian** has a query language (`tag:#a tag:#b path:wiki`) with no facets, no counts and no
  notion of a page's collection. Results are file paths with context lines.

An author who has learned the site's model of their repo (folders as collections, front matter
tags and inline hashtags as one tag set, chip combinations as the main way in) loses it when
they open Obsidian. This plugin puts the same model in the editor.

Three things follow from that:

1. **Same concepts in both places.** The plugin derives title, collections, categories, tags,
   rating and dates from the same front matter keys and conventions as the site, so it doubles
   as a rough preview of what the site will list. The aim is the same *kind* of answer, not
   identical results page for page.
2. **Facet combinations are a first-class way to browse**, not a query-syntax trick. You click
   chips to include or exclude, watch the counts change, and open a page.
3. **A step toward PublicTxt's aggregate view**: pulling pages from other people's repos into one
   local place to browse. This plugin would be the browsing part. It doesn't aggregate anything
   itself: pulling repos into the vault is a separate Obsidian plugin, in progress (2026-10-29),
   which keeps this one free of network access. Once that one is public, the README can link it
   as related; until then the README mentions aggregation only as an aim.

## Who it is for

- People who write a PublicTxt repository in Obsidian and publish it with PublicTxt-Pages.
- Anyone who wants faceted browsing of tags and folders, with cards. Nothing requires PublicTxt
  conventions. Each one is a default that a plain vault can live without or turn off.

## What success looks like

- Someone who knows the site's browse lists needs no explanation of the plugin's.
- The view opens in under a second on a vault of a few thousand notes. Clicking chips, sorting
  and **Show more** feel instant.
- Editing a note updates the results without reloading the plugin.

## What it is not

- **Not a replacement for core search.** No query syntax, no regex, no jumping to a match in a
  file. For now, no text search at all.
- **Not a writer.** It never changes the vault. Links, front matter and hashtags are the site's
  sync step's job.
- **Not a copy of the site.** It shares concepts and behaviour, not code, page-for-page results or
  styling. Colours come from the Obsidian theme.

## Decisions (2026-10-28)

1. **Relationship to the site.** It is a general facet browser that suits PublicTxt and shares
   PublicTxt-Pages' core concepts and features. Site parity is a source of ideas, not a gate.
2. **Collection, not type.** A page belongs to one or more collections, as on the site:
   - its top-level folder (an option; root-level pages get `page`)
   - each value of front matter `collections:`
   - `bookmarks`, when it has a `bookmark:` / `bookmarks:` URL

   Collections are case-insensitive and shown lower-case. The archive's single-valued *type* is
   gone.
3. **Facets: collection, category, tags, rating.**
   - **Chip states.** Each collection, category or tag chip is off, included or excluded.
   - **all/any toggle.** Each chip facet matches all or any of its included values. Excludes
     always remove pages.
   - **Categories** come from a list in settings. An empty list means any `category:` /
     `categories:` value counts. The site always uses a fixed list; "any value" is the plugin's
     cheap default for vaults without one.
   - **Tags** come from front matter, inline hashtags, or both (a setting, as in the archive).
   - **Rating** is front matter `rating:` 1–5. One chip at a time, meaning "this many stars or
     more", plus *unrated*.
4. **Sort order**, from the site: Newest, Oldest, Title A→Z / Z→A, Recently / Least recently
   updated, Top / Lowest rated. Unrated pages sort as 2.5. Default: Newest.
5. **Compact toggle**: one line per card (title, date, stars).
6. **No full-text search to start.** Facets first, no search engine and no dependency.
7. **Title filter.** A text box that keeps cards whose title contains the typed text
   (case-insensitive), combined with the facets. It's a cheap stand-in until text search arrives.
8. **Platform.** Cross-platform (desktop and mobile). Obsidian 1.13 or later (2026-10-29), for the
   declarative settings tab and its settings search. Developed on Linux, so no PowerShell-only
   scripts.
9. **Template tooling kept.** ESLint with `eslint-plugin-obsidianmd` flags what Obsidian's
   community-plugin review rejects, and the GitHub workflows lint on push and build releases from
   tags.

Not taken from the site for now: the year/month timeline filter, per-folder default filters
(`filter:`), and post sources.

## Design tensions

- **One set of concepts, two implementations.** The site derives page data in Python at build
  time. The plugin derives it in TypeScript at load time. Shared concepts are written down in
  the Pages repo (`docs/SPEC.md`, `docs/user/writing.md`). The plugin follows them where they
  make sense in a vault and notes where it doesn't.
- **Categories: fixed list vs. any value.** With an empty list, the plugin counts category values
  the site would ignore (with a build warning). That's the author's cue to fix the value or the
  list. Filling in the setting with the site's list brings them back in line.
- **Tag syntax.** Obsidian reads `#a/b` as a nested tag. The site (`scripts/hashtags.py`) reads it
  as the tag `a`. Pages will add nested tags, so the plugin follows Obsidian and the gap closes
  from the site's side. Until then, nested tags differ between the two.
- **Dates without Git.** The site can fall back to Git history for a page's dates. The plugin
  falls back to file times. Dates in front matter or the filename give the same result on both.
  This matters more now that Newest is the default sort.
- **No URL.** The site keeps the browse state in the query string
  (`?tag=a&tag-not=b&tag-match=any&sort=title`). The plugin keeps it in the workspace layout and
  offers commands that start from the note you're in.
- **Counts with "any".** On the site, an *all* facet's counts come from the current results. An
  *any* facet's counts leave out its own includes, because picking another value *adds* pages.
  This is easy to get wrong, so it's written down here.

## Related plugins

Checked 2026-10-28 against Obsidian's community list. **Facet Folders**
(`grouped-property-explorer`) is the only other plugin that talks about facets: a
tree navigator that shows properties and tags as virtual folders, edits front matter in bulk,
and groups properties in Bases. It has no combined filtering, no include/exclude, no counts
across facets, no cards, no sorting of results. Its README points at **Property Explorer**
for plain property browsing. PublicTxt Facets' niche is combinable, counted, read-only
filtering with cards.

## Open questions

1. **Text search later: Obsidian's or our own?** Obsidian's core search has no public API. A
   plugin can *open* it with a query, but only through undocumented internals, and it can't read
   the results back to combine with facets. So "integrate with Obsidian search" probably means a
   command that hands the current facets to core search as a query (`tag:#a -tag:#b`), not
   search inside this view. Search inside the view means our own engine: MiniSearch, or plain
   substring matching first.
2. **Does the title filter become the search box?** When text search arrives, the title filter's
   box is the natural place for it. Decide then whether title-only filtering stays as a mode.
3. **"Show pages like this one": all or any tags?** It matches the page's tags `all`, as the tag
   facet does, which for a page with many tags often finds little beyond the page itself. `any`
   finds more. Possibly a setting later (decided 2026-10-29: `all` for now).
