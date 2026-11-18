---
mirror_remote: publictxt-facets
mirror_repo: https://github.com/publictxt/obsidian-publictxt-facets
mirror_path: docs/spec.md
mirror_branch: master
mirror_commit: 11eb3c44b065e5040c2708e2da5ab5c6d38bd2fe
mirror_blob: 013c26efafd69509e3c1c30a6c9df70546242ecb
mirror_date: 2026-11-17T21:19:50+02:00
---
# PublicTxt Facets — Spec

What the plugin does. Why in `intent.md`, names in `naming.md`. The plugin never writes to the
vault.

## Pages

A **page** is any `.md` file except:

- files under an excluded folder (setting)
- housekeeping files, by exact name at any depth: `README.md`, `LICENSE.md`, `CONTRIBUTING.md`
- files with `publish: off` / `false`, which the site hides

`_index.md` files are pages.

## Page record

| Field | Rule |
|---|---|
| `title` | front matter `title`, else the H1 on the first content line (the first non-blank line after the front matter), else the file name with `-` and `_` as spaces. An `_index.md` takes its folder's name instead, with `-` as spaces (the root's stays `index`), as the site's sync step does |
| `collections` | top-level folder (setting; `page` at root) ∪ front matter `collections` (value or list) ∪ `bookmarks` if there's a bookmark URL. Lower-case, deduped |
| `categories` | front matter `categories` / `category` (value or list), case-insensitive. With a settings list: only listed values, in the list's spelling. With an empty list: any value, lower-case |
| `tags` | front matter `tags` and/or inline `#tags` (setting), via Obsidian's metadata cache. Lower-case, no `#`, deduped |
| `rating` | front matter `rating`, integer 1–5; else unrated |
| `created` | front matter `created` (or `date`), else `YYYYMMDD` / `YYYY-MM-DD` in the file name or a `/YYYY/MM/DD/` path, else file ctime |
| `updated` | front matter `updated` (or `lastmod`), else file mtime; never before `created` |
| `summary` | first 30 words of the body, without front matter or the title H1 (the first-line H1) |
| `bookmarks` | front matter `bookmark` / `bookmarks`, `http(s)` URLs only |

## Filtering

`BrowseState` is the whole view's state: `{ title, facets, rating, sort }`, where `facets`
holds a `FilterState` (`{ inc, exc, match }`) per chip facet. The chip facets are one table,
`FACETS` in `src/filter.ts`.

- **Title filter**: title contains the text, case-insensitive.
- **Chip facets** (collection, category, tag): a chip is off, included or excluded. Click the chip
  to include or clear it; click its ✕ to exclude or clear it. A page passes a facet if:
  - it has none of the excluded values, and
  - it has all of the included values (`match: all`) or any of them (`match: any`).
- **Default match**: collection `any`, category `any`, tag `all`. The all/any toggle shows once
  two or more values are included.
- **Rating**: one chip at a time, 5…1 or unrated. A number means "at least this many stars".
  Only the ratings some page has are offered; with no rated pages the group is hidden. It comes
  first in the filter column, as on the site's search page.
- All filters combine with AND.

### Counts

- **Chip order** follows totals across all pages (count descending, then name), so chips don't
  move.
- **Chip number** is the count within the results of every *other* filter, plus:
  - for an `all` facet, its own includes
  - for an `any` facet, *not* its own includes, since picking another value adds pages
- Excluded chips show no number. Zero-count chips are dimmed, not hidden.
- A facet shows its first 20 chips, plus any that are set; "+N more" shows the rest.
- A facet with no values at all (categories, in a vault that doesn't use them) is hidden.
- **Rating chip number**: pages with at least that rating, counted without the rating filter.

## Sort

`created` (Newest, default), `created asc` (Oldest), `title` (A→Z), `title desc`, `updated`
(Recently updated), `updated asc`, `rating` (Top rated), `rating asc`. Unrated pages sort as 2.5.
Ties go to title A→Z (to path, when sorting by title). The menu is in the bar; Clear returns it
to Newest.

## View

- One `ItemView` (`publictxt-facets`), reused. It opens as a tab from the ribbon or the **Open**
  command.
- **Top**: title filter, sort menu, **Compact** toggle, **Clear**.
- **Status line**: `N pages — <filters>`.
- **Filters column**: one group per facet, right of the results. It goes above them when the
  view is narrower than 640px.
- **Cards**: title, collections, dates, rating, summary, tag chips, bookmark ↗. Compact cards are
  one line: title, date, stars.
- **Opening**: a card's title opens the note. Ctrl/Cmd-click opens it in a new tab, and hovering
  shows Page Preview.
- **Card tag chips** include or clear that tag.
- **Batches**: 20 cards, then **Show more**.
- **Saved state**: `BrowseState` and the compact toggle are saved with the workspace layout,
  as `{ browse, compact }`. The layout is saved when the user changes them, never on index
  updates. A malformed saved state falls back part by part. A set value no page carries still
  shows its chip (count 0), so it can be cleared.
- **Live updates**: note create, change, rename and delete update the results. Each path waits
  300 ms after its last event, then is re-read, or dropped when it's no longer a page (deleted,
  moved into an excluded folder, `publish: off`). Folders go by prefix. The view re-runs once per
  burst and keeps as many cards shown as before, and its scroll position.
- **Rebuilds** read every page and swap the new index in whole; the old one stays on screen
  meanwhile. A newer rebuild stops an older one.

## Commands

| id | Name |
|---|---|
| `open` | Open |
| `filter-tag-under-cursor` | Filter by this tag (the tag under the cursor) |
| `like-this` | Show pages like this one (its collections and tags, included) |
| `clear` | Clear |

- **Filter by this tag**: the tag under the cursor, an inline `#tag` or an entry of front matter
  `tags:`. Offered only when the cursor is on one.
- **Show pages like this one**: offered only when the active note is a page. Its tags match
  `all`, as the tag facet does by default.
- Both open or focus the view and **replace** its browse state, keeping the sort.
- **Clear** is offered while the view is open.

## Settings

| Setting | Default |
|---|---|
| Excluded folders | none (Obsidian never indexes dot-folders) |
| Top-level folder as a collection | on |
| Use first top level heading as title | on |
| Tags from | front matter and inline |
| Use categories | on |
| Categories list (shown while categories are used) | empty (any value) |
| Use ratings | on |
| Open notes in | current tab |
| Cards per batch | 20 |

The tab is declared (`getSettingDefinitions`, Obsidian 1.13+), so its settings appear in
Obsidian's settings search. Every change goes through `normalizeSettings`, as `data.json` does.
**Use categories** off: pages carry no categories, so the category facet is gone. **Use ratings**
off: every page is unrated, so the rating group, card stars and the two rating sorts are gone.
The view drops a saved filter or sort for either, so nothing hidden narrows the results.

**Use first top level heading as title** off: a page with no `title:` takes its file name, and its
first-line H1 stays in the summary.

Excluded folders, folder as a collection, heading as title, tags from, both toggles and the categories list rebuild
the index (the text areas once typing pauses). Excluded folders are one per line; categories one
per line or comma-separated.

## Build order

Each slice is usable in the test vault (`test-vaults/txt-cpy`) before the next starts.

1. ✓ Scaffold: names applied, an empty view opens, a deploy script.
2. ✓ Page records (`src/page/`, with tests), plus a plain list sorted by Newest.
3. ✓ Cards and compact mode.
4. ✓ Chip facets: include/exclude, all/any, counts. Title filter.
5. ✓ Rating and the sort menu.
6. ✓ Live updates, saved state, settings tab, commands.
7. Release 1.0.0: user README, release workflow tests and lints, submission to the community
   directory.
