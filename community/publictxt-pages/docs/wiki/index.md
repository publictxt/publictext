---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/wiki/index.md
mirror_branch: main
mirror_commit: 184a8535c03384a65b94a3ef851e0127c1b99085
mirror_blob: 440b6b936122ee52ff781c4523bb392b388147f6
mirror_date: 2026-11-25T22:44:54+02:00
---
# Map

Every source file, one line — to find the right files, then read those: every script
has a docstring, every partial a `{{/* */}}` contract. Code wins any disagreement.
[traps.md](traps.md) holds what spans files; **read it before editing templates or the
pipeline.** [SPEC.md](../SPEC.md) is what the site *should* do.

Add, move or remove a file: update this map. `python scripts/map_lint.py` checks both ways —
a bare name counts only under its folder's heading.

## Python — pipeline (no dependencies; 3.10+)

Hugo never reads the source repo — only what sync writes: `build/content/`, `build/site.toml`.

```txt
scripts/sync_content.py     source repo -> build/content: titles, dates,
                            hashtags, source_path, folder notes (`is-folder-index`) -> _index.md, skips (incl. `publish: off`, `params.excludeFolders`, `params.skipFiles`);
                            reads `params.titleFromHeading`; a fork's or a mirror's source_* keys, a follow's `follow_name` and `url` -> `follow_url`;
                            `in_community` (`params.communityFolder`); links to pages not mirrored -> the original
scripts/sources.py          follows, forks, mirrors and the join: a fork's or a mirror's source, source rating, link to the original;
                            follows' mirror folders
scripts/dates.py            created/updated ladders, one git log pass, sort_key
scripts/hashtags.py         HASHTAG_RE; linkify (body), merge_tags (front matter `tags`); nested tags + parents, flat keys
scripts/build.py            full pipeline, any OS, + search index check; docstring has usage
scripts/build.sh            shim -> build.py, for deploy workflows copied before it
scripts/cloud-setup.sh      installs latest standard Hugo + pinned Pagefind on Linux (cloud env setup script)
scripts/map_lint.py         this map vs the tree; docstring: how it reads the map
```

## Tests — `python -m unittest` (stdlib), `node --test "tests/js/*.test.mjs"`

```txt
tests/test_golden.py        example/txt through sync vs tests/golden/;
                            docstring: refreshing the snapshot
tests/test_dates.py         the date ladder, which the golden test fixes
tests/test_title.py         derive_title: the H1 on the first line only; titleFromHeading
tests/test_folder_index.py  `is-folder-index: true`: a note as its folder's _index.md; links to it rewritten
tests/test_exclude.py       params.excludeFolders (whole subtrees) and params.skipFiles, left out of sync
tests/test_sources.py       sources.py: canonical URLs, follows, the join; what sync writes from it
tests/test_hashtags.py       merge_tags: a `#` already in front matter `tags` is dropped; nested tags
tests/test_site.py          that content through Hugo: index.json vs tests/golden/, Pagefind keys on every page (needs hugo)
tests/test_build.py         build.py's search index check, on made-up public/ trees
tests/test_map_lint.py      map_lint.py's reading of the map: folder headings, namesakes
tests/js/facets.test.mjs    facets.js + sorts.js: URL spelling, chip presses, lists = search
tests/js/cards.test.mjs     cards.js: remembered density, storage that throws
tests/js/page-size.test.mjs page-size.js: remembered size, choices, keeping place
tests/js/timeline.test.mjs  timeline.js: year/month grouping, ?month= spelling, tag chips
```

## Templates

```txt
layouts/baseof.html               shell: header + ☰, nav | main | "aside" block;
                                  data-base + data-index on <html>
layouts/home.html                 hero, Recent list; aside: its filters
layouts/page.html                 single page; data-pagefind-body; aside: page meta, then
                                  folder tree (treeSections) or timeline
layouts/section.html              index body as prose, folder tree (treeSections) and/or
                                  timeline (timelineSections), browse list; aside: its filters
layouts/term.html                 /tags/<term>/; aside: its filters
layouts/taxonomy.html             /tags/
layouts/search.html               search UI shell, filters in its aside; search.js fills it
layouts/404.html

layouts/_partials/
  head.html             title, description, favicon, Mastodon rel="me", stylesheet, nav.js
  sidebar.html          left nav: sections / categories / tag cloud
  page-meta.html        "This page": a page's meta, in its right column
  page-timeline.html    below it: a timeline with titles — timelineCollections', or the section's; timelineTags
  breadcrumbs.html      .Ancestors trail
  footer.html           [params.footer] wording; edit-in-repo link
  crumb-label.html      a page's short label; every folder label: its name, as cased
  sections.html         top-level sections in sectionOrder   (partialCached)
  recent.html           the one "recently updated first"
  page-date.html        created, + updated when shown
  date-labels.html      the one date form + >1-day rule, for page, sidebar, JSON, Pagefind
  month-names.html      January..December in the site's language, for JS and templates (returns them)
  tag-cloud.html        weighted term chips
  site-index.html       publishes index.json, returns URL    (partialCached)
  list-json.html        pages -> index.json items, with Hugo-made display values
  list-container.html   [data-list] + no-JS <ul> + loads list.js
  js-params.html        @params for every js.Build — see traps.md
  chip-facets.html      only reader of params.chipFacets; appends `source` and `copy`, in the Community block   (partialCached)
  list-order.html       order: cascade (page -> ancestors -> param)
  list-filter.html      filter: cascade (page -> ancestors): default chip filters
  list-per-page.html    pagerSize: cascade
  hidden-collections.html  only reader of params.hiddenCollections: out of lists and search by default
  pagefind-keys.html    hidden sort keys + filters — see traps.md
  bookmark-urls.html    only reader of bookmark:/bookmarks:
  bookmark-links.html   the chip row
  page-collections.html only reader of collections:; + section, "bookmarks" if URL, "community" if in_community
  collection-pages.html every page of a collection        (partialCached)
  section-pages.html    what a section lists, + its scope
  bookmark-label.html   one URL's short form (returns it)
  posted-urls.html      only reader of params.sources keys (facebook:, …)
  posted-links.html     "Posted on" chips
  follow.html           a follow page's repo, rating (the repo's), description, fork and mirror counts (returns a dict)
  sources.html          every follow by name, for cards' source chips (@params)   (partialCached)
  fork-source.html      only reader of the source_* keys sync writes on a fork or a mirror (returns a dict)
  own-source.html       only reader of params.ownSource: the Source filter's value for this repo's own pages
  fork-source-links.html  "Forked from" / "From" chips: the source, its follow's page, the original
  categories.html       only reader of categories:/category:
  category-counts.html  used categories + counts, for the sidebar (partialCached)
  rating.html           only reader of rating: — but a follow's, which is follow.html's
  note-embed.html       ![](page.md) -> the page's content boxed; inline/unresolved -> link
  attachment-url.html   relative file path -> its URL: beside the note, or a top-level folder (returns it)

layouts/_partials/tree/   a folder tree of a tree section (params.treeSections)
  enabled.html          only reader of treeSections: is a page or section in one?
  aside.html            a page's, in place of its timeline: cached, the page's path marked in
  content.html          a folder's, in its content: its subtree, one level open
  node.html             one folder's rows, recursing, all shut (partialCached, per folder)
  row.html              a row's opening tags, every form: shut, "on" (path), "open"
  mark.html             one row of a cached tree opened or marked, by row.html's forms

layouts/_partials/timeline/   a folder's pages by date, in its content (params.timelineSections)
  enabled.html          only reader of timelineSections: is a section in one?
  content.html          years, months, titles: timeline.js's archive markup, server-rendered

layouts/_markup/
  render-image.html     every ![](…): .md -> note-embed; YouTube -> iframe; audio -> <audio>; else <img>
  render-codeblock-dataview.html  ```dataview LIST FROM #tag -> compact list, else code
```

## Browser

```txt
assets/js/site-index.js   fetch index.json (recent-first) once per document; scope() subsets
assets/js/layout.js       breakpoints for JS; dock(): filters right column <-> page flow
assets/js/nav.js          ☰: collapse the nav (wide), drawer (narrower); blocking, in <head>
assets/js/sorts.js        the sort vocabulary, ?sort= spellings
assets/js/cards.js        the one card renderer; rating filter value; density toggle
assets/js/page-size.js    lists' page size: the reader's remembered pick, its choices
assets/js/facets.js       chip facets from config; include, exclude, match any/all; rating
                          chips; remembered section folds — shared by lists and search
assets/js/list.js         browse list: state<->URL, facets, paging
assets/js/timeline.js     the timeline: pages by year/month; picks the date filter, or archive links + titles
assets/js/page-timeline.js  a page's archive timeline, the page marked; collection chips when shared, timelineTags chips
assets/js/search.js       Pagefind UI: filters, sort, incremental results
assets/css/main.css       the whole theme; palette + column widths in :root
```

ES modules bundled per entry point (list.js, search.js, page-timeline.js; nav.js as a classic script) by
Hugo's built-in esbuild — no Node to build. One sort vocabulary, one card and one facet
module serve lists and search; see traps.md.

## Config and delivery

```txt
hugo.toml                            mounts, front matter mapping, params (incl. skipFiles default)
deploy/publish-to-github-pages.yml   template for the CONTENT repo; header has setup
.github/workflows/test.yml           tests + map_lint on push and PR
site-content/                        site-owned pages (search)
example/txt/                         default source repo + golden-test fixture;
                                     its README maps case -> file
static/                              favicons, logo
```

**Generated, never edit** (gitignored): `build/content/`, `public/`, `resources/_gen/`.
