---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/user/site.md
mirror_branch: main
mirror_commit: 8dde109855f4ca8323ea404f8327a48c1f497d3c
mirror_blob: d1c18ac088a5d3c6f3cf4eaaa292b884483cff85
mirror_date: 2026-11-17T21:07:18+02:00
---
# The published site

What your readers get — and what your writing choices turn into.

- [Layout](#layout)
- [Home](#home)
- [Section pages](#section-pages)
- [Browse lists: sort and filter](#browse-lists-sort-and-filter)
- [The list timeline](#the-list-timeline)
- [On a page](#on-a-page)
- [Tags](#tags)
- [Search](#search)
- [Phones, and no JavaScript](#phones-and-no-javascript)

## Layout

Three columns, centred on wide screens:

| Left: navigation | Middle: content | Right: context |
|---|---|---|
| Sections, categories with counts, tag cloud | The page or list | On a page: its details and timeline or folder tree. On a list: filters and timeline |

**☰** in the header collapses the left column on wide screens (remembered), or opens it
as a drawer on narrower ones.

## Home

Your root `_index.md` (or, without one, the site description), then **Recent**: every
page, newest first ([`listOrder`](community/publictxt-pages/docs/user/configuration.md#browse-lists)), with the full filters and timeline beside it.

## Section pages

`/blog/`, `/wiki/`, and every sub-folder:

1. The folder's `_index.md` text
2. For a [tree section](community/publictxt-pages/docs/user/configuration.md#timeline-and-folder-tree) (default `wiki`): its
   sub-folders, one level open
3. The browse list — for a top-level section, every page in its
   [collection](community/publictxt-pages/docs/user/writing.md#collections), wherever filed; for a sub-folder, only what's
   filed under it

Breadcrumbs above every page but home trace the folders.

## Browse lists: sort and filter

Home, section and tag lists share one interface.

**Sort**: Newest, Oldest, Title, Recently updated, Top/Lowest rated (unrated counts as
2.5). Starts at the [configured order](community/publictxt-pages/docs/user/configuration.md#browse-lists).

**Show**, beside sort: cards per page — 10, 20, 50 or All, plus the list's own
[default](community/publictxt-pages/docs/user/configuration.md#browse-lists). Changing it keeps the cards you were looking at
on screen. Remembered in your browser for every list; not part of the URL.

**Compact**, beside them: one line per page — title, date, stars — for skimming. Remembered
in your browser for lists and search; not part of the URL.

**Filter chips** — Collection, Category, Tags (as [configured](community/publictxt-pages/docs/user/configuration.md#filter-chips)):

- Click a chip to **include** it; click again to clear.
- Click its **✕** to **exclude** it (shown on hover, focus, or once set).
- Several included: **all** (pages with every one) or **any**, via the toggle.
- **Rating** chips: one at a time, each meaning "this many stars or more".
- Each filter group folds; a folded group shows how many values are picked. Folds are
  remembered across lists and search.

**Every list state is in the URL** — sort, chips, page, date. Copy the address to share
exactly what you see.

## The list timeline

Below the filters, on lists spanning two or more months:

- Years on a rail, each with a Jan–Dec sparkline; the newest open.
- Open a year for its months, with bars and counts.
- Click a year or month to filter the list to it; the other filters still apply.

Dates here are `created` — see [Dates](community/publictxt-pages/docs/user/writing.md#dates).

## Community

When a list holds pages from more than one source — your own and
[mirrors and forks](community/publictxt-pages/docs/user/writing.md#follows-and-forks) from repos you follow — a **Community** block
appears under Filters, in lists and in search:

- **Source** — a chip per source: click to show only those, ✕ to leave one out. Your own
  pages are "this repo" ([`ownSource`](community/publictxt-pages/docs/user/configuration.md#filter-chips) renames it).
- **Copy** — **forked** (your version of a page) or **mirrored** (the page as its author
  wrote it): click to show only those, ✕ to leave them out (`?copy=forked`,
  `?copy-not=mirrored`). Your own pages are neither, so picking one leaves them out.
- **Sort by source** — *Top rated source* / *Lowest rated source*, in the Sort menu where
  a list has mirrors or forks. Your own pages count as above any source.
- **Source rating** — your rating of the repo (the follow's `rating`), as a minimum:
  ★4+ shows mirrors and forks from repos you rated 4 or 5. "Unrated" is those from repos
  you haven't rated. Your own pages have no source rating, so picking one leaves them out.
- A mirror's or a fork's card says **from** *source* — a link to the follow's note; hover for the
  repo, your rating and the follow's description. Its page shows the source's rating too.

Pages under `community/` — mirrors: pages from repos you follow, as their authors wrote
them — are left out of lists and search to start with: **community** shows crossed out
under Collection; click it to bring them in. The Community section itself always lists
them. Forks live in your own folders and are yours to show, and do. ([`hiddenCollections`](community/publictxt-pages/docs/user/configuration.md#browse-lists).)

## On a page

Right column, top: **this page's details** —

- collections, created and updated dates, categories, rating stars, tags
- the bookmark link, if any; "Posted on" links to where it was cross-posted
- for a [mirror or a fork](community/publictxt-pages/docs/user/writing.md#follows-and-forks), its source and a link to the original

Below that, **where it sits**:

- **Timeline** — the page's section, by date, with titles; its year and month open, the
  page marked; year and month labels link to the list at that date. Pages in
  [`timelineCollections`](community/publictxt-pages/docs/user/configuration.md#timeline-and-folder-tree) (default `blog`,
  `posts`) share one timeline, with a chip per collection — and per `timelineTags` tag —
  to narrow it.
- **Folder tree** — for tree sections (default `wiki`): folders then pages, A→Z, open
  along the page's path. Reference material is read by place, not date.

Root-level pages get neither.

Footer: **Improve this page** opens the note in your repo's GitHub editor (if the
workflow sets `HUGO_PARAMS_EDITURL`).

## Tags

- `/tags/` — every tag.
- `/tags/<tag>/` — a browse list of that tag's pages, with filters.
- Inline `#hashtags` in a page link to their tag page.

## Search

The **Search** page: full text over every page and section intro, with:

- Collection, Category, Tag and Rating filters — usable with no query, to browse
- Year and month from the timeline
- The same sorts as browse lists

*Show more* loads further results, a batch at a time ([`pagerSize`](community/publictxt-pages/docs/user/configuration.md#browse-lists)). Search runs in the browser ([Pagefind](https://pagefind.app));
nothing is sent to a server.

## Phones, and no JavaScript

- **Phones**: one column. Filters fold above the results (open when one is set); a
  page's details move below it.
- **No JavaScript**: pages, navigation and the folder tree all work; lists fall back to
  plain links without filters; search needs JavaScript.
