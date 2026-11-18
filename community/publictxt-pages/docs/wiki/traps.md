---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/wiki/traps.md
mirror_branch: main
mirror_commit: 8dde109855f4ca8323ea404f8327a48c1f497d3c
mirror_blob: a9451a88ee9a52a24062215f3fdc13ff6f2f0562
mirror_date: 2026-11-17T21:07:18+02:00
---
# Traps

Cross-file invariants — each is otherwise documented only in a file you'd have to
already know to open. **Read before editing templates or the pipeline.**

## Pagefind drops pages silently — `build.py` fails the build instead

Every `data-pagefind-body` template **must** call `pagefind-keys.html` (now:
`page.html`, `section.html`). Pagefind drops a page lacking
the key it sorts on, or the filter selected — so a template without it vanishes from
sorted and filtered results, no error. Hence the rating sort key and filter are on
every page, rated or not. `check_search_index()` catches a page without them, and an
index count that differs from the body pages; `hugo server` builds skip it.

## Pairs that must change together

Display values (date labels and their `arrived` word — "forked", "mirrored" —, year, bookmark labels, recent order) are made once, in Hugo,
and reach JS as data: `index.json` for lists, Pagefind meta (`pagefind-keys.html`) for
search. Add one to both, or search cards go without it. What's still implemented twice:

| Server | Client |
|---|---|
| each list template's page collection (incl. the dataview hook) | its kind in `scope()` |
| stars in `page-meta.html` | `ratingHTML()` in `cards.js` |
| unrated sort value 2.5 in `pagefind-keys.html` | `UNRATED` in `sorts.js` |
| own pages' source rating sort value 6 in `pagefind-keys.html` | `OWN_SOURCE` in `sorts.js` |
| `source-rating` `"none"` on this repo's own pages, in `pagefind-keys.html` | `NO_SOURCE` in `cards.js`; `sourceRatingPasses()` is the lists' rule, `pagefindFilters()` search's |
| `created`'s `2006-01-…` form in `list-json.html` | `monthOf()` in `timeline.js` reads its digits |
| Pagefind `month` filter, `"2006-01"`, in `pagefind-keys.html` | `countsTree()` in `timeline.js`, `?year=&month=` → `month` in `search.js` |
| `timeline/content.html`: grouping (year, month, newest first, undated out, ≥2 months) and markup (`.tl-*` classes; no sparklines or month bars) | `timeline()`, `monthCount()` and `renderTimeline()` in `timeline.js`; `main.css` styles both |

A nested tag's `/` is `--` in its term key and URL: `term_key()`/`tag_path()` in
`hashtags.py` (sync also writes its term page's title), `tagURL()` in `cards.js`, the
`$key` in `render-codeblock-dataview.html`. Hugo nests a term with `/` under its
parent and misreads the parent; templates and JS use a term's `.LinkTitle` (its name),
never `.Data.Term` (the key).

A chip facet key (`BUILTIN` in `facets.js`, `chip-facets.html`) is the URL param **and** the Pagefind filter
name (`data-pagefind-filter` in the page templates, `pagefind-keys.html`); rename all
together.

`source` is a chip facet no config lists: `chip-facets.html` appends it, with `group:
"community"` — its own block, built by `list.js` and written out in `search.html` — and
`missing`, the value of this repo's own pages (`own-source.html`). `index.json` leaves
`source` off those pages and `facetValues()` fills it in; Pagefind can't, so
`pagefind-keys.html` writes it on every page. Both read `own-source.html`.

`copy` ("forked", "mirrored") follows it in the same block, with no `missing`: this
repo's own pages carry no value in `index.json` or Pagefind. It is not `arrived`, the
date's word, which only an undated fork or mirror has (`date-labels.html`); both come
from `fork-source.html`'s `mirror`.

`params.hiddenCollections` is hidden three times over, by one rule — a list with some
pages of the collection, not all: `list-container.html` (the no-JS list), `list.js` and
`search.js` (the default chip state, `hideByDefault()`). Anything else that lists pages
(dataview's compact lists go through `list.js`; timelines and trees don't) shows them.

A URL omits `-match` at the facet's default (`params.chipFacets` `match`, first
entry): change the default and shared links without it change meaning.

The Pagefind version is pinned in `build.py` (`cloud-setup.sh` reads it); `search.js` is written against its JS
API, so a bump means checking search too.

Also: the breakpoints in `main.css` (1440px drawer, 900px one column) are repeated
in `layout.js`, JS's only copy.

## Three JS bundles, one `@params`

`js.Build` bundles each entry point (`list.js` from `list-container.html`, `search.js`
from `search.html`, `page-timeline.js` from `page-timeline.html`) with its imports, so
shared modules are **copied into each**. (`nav.js`, from `head.html`, is a fourth
bundle; it takes no `@params`.) Anything set per call — options, `params` — must match
across them, or lists, search and pages drift silently. Hence `js-params.html`, the one
source of `@params`.

## The right column is filled from both sides

A template's `{{ define "aside" }}` is its right column; none, no column. List filters
are JS-only: `section.html`/`term.html` emit an empty `[data-list-controls]` aside that
`list.js` claims and fills; search's filters are server-rendered in `search.html`'s aside.
Below 900px, `dock()` (`layout.js`) moves them into the page, and `main.css` hides the
aside by `:empty` — so these asides must hold no whitespace (`{{- -}}`) around their child.

## Search counts run higher than list counts

Pagefind also indexes section index bodies (`section.html`); browse lists hold regular
pages only. `category-counts.html` counts sections too, to match the search it links to.

## `hugo.toml`: a key after a `[params.x]` table belongs to it

TOML ignores indentation. `sectionOrder` and four others sat below `[params.footer]`
and were silently `params.footer.*`. Plain `[params]` keys go above the first table.

## `partialCached` on site-scanning partials

`site-index.html`, `sections.html`, `collection-pages.html`, `category-counts.html`
walk every page; uncached, the build goes O(pages²). So does `tree/node.html`
(cached per folder, in `tree/aside.html`, `tree/content.html`) across a tree section's pages.

## Build order and `public/`

`sync → hugo → pagefind`, each reading the last one's output. Remove
`public/` before `hugo` — it keeps stale pages. `build.py` does this.

## `fetch-depth: 0` in CI

Dates come from first/last commits; the default one-commit clone gives every page
the same date. Sync warns — check the log if dates look wrong.

## Front matter handling is not a YAML parser

Sync edits front matter line by line (`tags` in `hashtags.py`); nested or multi-line
keys are invisible. Swap in a real parser rather than extend the regexes.

## Hashtag regex alternation order is the logic

`HASHTAG_RE`: linkified tags, then protected regions (code, links, URLs), then bare
tags. Reordering changes every page's tags.

## Sync quirks

- `normalise_md` drops blank lines from existing front matter.
- `*.md` globs match folders too — bookmark domain folders (`sites/obsidian.md/`).
  Check `is_file()`.

## Search is unavailable under `hugo server`

The Pagefind index comes from a full build; the search page says so. `hugo server`
watches `build/content`, so content changes need sync re-run.

## Fingerprinting is production-only

`head.html`, `list-container.html`, `search.html`, `page-timeline.html`: fingerprint + `integrity` only when
`hugo.IsProduction`.
