---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/SPEC.md
mirror_branch: main
mirror_commit: 8dde109855f4ca8323ea404f8327a48c1f497d3c
mirror_blob: 0301c543ca6e0b5eaa96876e37dff997c09250f0
mirror_date: 2026-11-17T21:07:18+02:00
---
# Spec

What the site does and should do. How it works lives in [`docs/wiki/`] ( that before the source)and the source; settings and defaults in `docs/user/`.

Items marked **(TBD)** are not built.

## Feature list

- Build operates on generated copy `build/content/`.
- Sections
  - Top-level folders are sections (`wiki/`, `blog/`, `notes/`, `bookmarks/`, `posts/`, …). Root-level `.md` → plain pages.
  - A folder's index (section page) is `_index.md` — at any depth; the root's is home. Its body renders as prose and is search-indexed.
  - The sidebar lists sections by folder name, in `params.sectionOrder`. (TBD-ISSUE)
  - Top-level sections list every page of their [collection](#collections), wherever filed.
  - Contents views below a section's index body (server-rendered, not search-indexed):
    - Folder tree for `treeSections` (default wiki).
    - Timeline by date for `timelineSections` (default blog, posts) once the list spans two or more months.
  - Sections move to specialised Views over Collections (TBD)
    - Blog view: timeline-like, toggling collections (a page's shared timeline already does this for `timelineCollections`).
    - Bookmarks view.
    - Wiki view: a right-column panel of categories, beside the folder tree.
    - Panels per section, not one view each: a section lists its right-column panels in order (e.g. `panels: [tree, timeline]`), cascading from its `_index.md`. `timelineCollections`/`timelineTags` and `treeSections` would fold into it.
    - Non-log timelines (wiki, notes) may want `updated`, not `created`, as their date.
- Layout: left sidebar, content, right sidebar, centred beyond a max width. Header ☰ toggles the left sidebar. One column on phones. Works without JS.
- Left sidebar: sections, categories, tag cloud.
- Right sidebar — context for what's shown
  - Page meta (collections, dates, categories, tags), then a timeline of the page's neighbours with the page marked. Scope: pages of `timelineCollections` (default blog, posts) share one, with collection chips and `timelineTags` chips to narrow it; other pages get their section's; root-level pages none.
  - Pages of `treeSections` get their section's folder tree instead — folders only, so a page filed elsewhere with `collections: wiki` isn't in it.
  - On browse lists and search: the filters, then (lists) a timeline.
  - Author (TBD) — carried in front matter, rendered nowhere.
- Categories
  - `categories:` (or `category:`) from a closed list in `hugo.toml`; feature toggleable.
  - Category pages (TBD)
- Tags
  - Inline `#hashtags` merged into `tags`.
  - Nested tags (`#dev/app`) also tag the page with their parents (`#dev`), so a parent includes its children, as Obsidian's search does.
  - Tag pages.
  - Tag and category facets: include or exclude values; match all or any (default all). One mode per facet — no mixed groups like `(a OR b) AND c` (TBD if needed).
- Browse lists
  - Home *Recent*, sections and tag pages render client-side from one site-wide JSON index, with a plain `<ul>` no-JS fallback.
  - Home *Recent*: every page, newest first (`listOrder`).
  - A folder's default filters (`filter:`, e.g. blog without `#journal`); Reset returns to them.
  - Sort by Date, Recency, Alphabetical, Rating, Source rating (only where there are forks). Sort by Source/Author (TBD).
  - Community filters: Source, Source rating — see Sources.
  - Filters: Collection, Category, Tag, Rating (`rating:` 1–5, minimum), and Year/month picked from the timeline.
  - Compact toggle for one-line cards.
- Search
  - Full-text search via Pagefind.
  - Same facets and sorts as browse lists, usable without a query. Tags past the first 20 fold behind "more", as in lists. Date ranges (TBD).
- Breadcrumbs on all pages but home, from folder names.
- Bookmarks
  - A page with a `bookmark:`/`bookmarks:` URL is a bookmark wherever it lives; pages under `bookmarks/` are too.
- "Posted on" links
  - A URL under a `[params.sources]` key (`facebook:`, `twitter:`, `substack:`, `mastodon:`, `github:`) shows as a "Posted on" link.
  - Filter by where it was posted, "Posted on" chips on cards (TBD).
  - Pages with "Posted on" links join the Posts collection (TBD).
- Sources — the PublicTxt spec's terms (§3.2, §5.2, §5.4, §5.5)
  - A follow is a page in `follows/` with a repo `url` (or `local: true`, for a repo with no public URL); its file name is its name, its `rating` this author's rating of that repo.
  - A fork (`fork_*` keys) has a source: the follow `fork_remote` names, else the one whose `url` is `fork_repo`; with no follow, that name or URL. Its source rating is the follow's `rating`, else unrated. Every other page is this repo's, with no source rating.
  - A mirror (`mirror_*` keys, and no valid `fork_*`) has a source by the same rules, with `mirror_remote` and `mirror_repo`. It is the followed repo's page as its author wrote it; a fork is this author's version. It shows "From" where a fork shows "Forked from", and wherever below says fork, a mirror counts too. Its own `rating` is its author's.
  - Computed at sync, never read from the repo. A fork shows "Forked from": the source, linked to the follow's page, and a link to the original — the forked file at the commit it was forked from (GitHub, GitLab, Codeberg), else the repo, else nothing (a local follow).
  - A fork whose `fork_remote` names no follow, or a mirror whose `mirror_remote` names none: build warning.
  - Community: a collapsible block of its own in lists and search, after Filters, for filters on where pages came from. Hidden with nothing to offer — a site, or a list, with one source.
    - Source: include / exclude chips, any of the included; this repo's own pages are `params.ownSource` ("this repo"). Not in `params.chipFacets`: always on.
    - Copy: include / exclude chips, "forked" and "mirrored", any of the included (`?copy=forked`, `?copy-not=mirrored`). This repo's own pages are neither, so an include leaves them out. Always on, as Source; shown where a list has some forks or mirrors, not only those of one kind.
    - Cards of forks show "from *source*".
    - Source rating: a minimum, as Rating (`?source-rating=4`), over forks; "Unrated" is forks whose source has no rating. This repo's own pages have no source rating, so any value leaves them out. Shown when forks' source ratings vary. A fork's page shows it under Source.
    - Sort by source rating: "Top rated source" puts this repo's own pages first, above any source, then forks by their source's rating, unrated as 2.5 (mid-scale, as Rating's sort); ties newest first. Author (TBD).
    - A card's source chip links to the follow's page; its tooltip gives the repo, the source rating and the follow's `description:`.
  - `params.hiddenCollections` (default `community`, where mirrors live: `community/<follow>/<path in its repo>`): excluded by default in the Collection filter of lists (home, sections, tags, dataview) and search, so the excluded chip — or the collection's own section — brings them back, and Reset hides them again. A fork lives in this repo's own folders, not in `community`, so it shows. No feeds yet to keep them out of.
  - A follow's page is its source's landing page: page details show the repository (linked; "local repository" for a local follow), the source rating, and how many pages were mirrored and forked through it, each a link to search by source and copy (`?source=ann-notes&copy=mirrored`). Its `rating` is the repo's, so the page itself is unrated in lists, search and the Rating filter.
  - A fork's dates follow the usual ladder, so `created:` or a date in the file name carried over from the original holds. Without either, the dates are this repo's — when it was forked and last changed here — as the original's history isn't in this repo; such a date shows as "forked <date>", or "mirrored <date>" for a mirror (page, its details, cards). TBD: the forking tool could write `created`/`updated` from the source's history.
- Pages
  - Folders are sections and notes are pages, whatever a folder holds. A post folder (a note + attachments) is a section with one page; attaching beside the note needs no folder.
  - `publish: off` keeps a page — and a folder's attachments when all its Markdown is unpublished — off the site. Hidden, not private: it stays in the source repo.
  - `params.excludeFolders` leaves whole folders out, likewise hidden not private. Links into them dangle.
  - `![alt](youtube-url)` embeds the video; `![alt](file.mp3)` (and other common audio) a player.
  - Relative media must sit beside the note or in a top-level media folder; otherwise it's left as written, with a build warning.
  - `![](page.md)` on its own line embeds that page, one level deep; inline it's a link. `#heading` section embeds (TBD).
  - A ` ```dataview ` block `LIST FROM #tag` (optional `LIMIT n`) embeds that tag's pages. Other Dataview queries stay code; multiple tags, `WHERE`, `SORT` (TBD).
- Date properties: convert YYYYMMDD to the required format (TBD).
- Chips show tags, categories and collections. Facet chips toggle include / exclude / off (`params.chipFacets`): click to include (again to clear), ✕ to exclude.

## Content Structure

```txt

_index.md, About.md                     (home + root pages)

wiki/_index.md, wiki/A/_index.md        (folder index = section page (any depth))

blog/2023/12/17/x.md                    (dates derived from path)
blog/20260509-title.md                  (YYYYMMDD prefix date)
blog/2024/09/20240922-title.md          (YYYYMMDD prefix date)
blog/20260922-title.md & blog/image.png  (attachment beside the note)
blog/Post-Name/title.md & image.png     (post folder: a section with one page)

bookmarks/sites/domain/page.md     (one file per resource; needs `bookmark:` URL)
bookmarks/wiki/topic.md            (pages about bookmarks; no URL needed)

posts/post-name.md
notes/note-name.md

media/                             (non-Markdown copied verbatim)

params.skipFiles (CNAME, .gitignore), .git/   (skipped during sync)

```

## Collections

A page is in one or more collections. They show as chips on the page and its cards,
drive the Collection filter in lists and search, and decide which top-level section
lists it.

| Source                          | Collection it adds   |
| ------------------------------- | -------------------- |
| top-level folder it's filed in  | that folder's name   |
| `collections:` (value or list)  | each value           |
| `bookmark:` / `bookmarks:` URL  | `bookmarks`          |

- **Adds, never replaces** — the folder's collection always stays. Root-level pages get `page`.
- **Case-insensitive** — `Notes`, `notes`, `NOTES` are one collection; shown lower-case.
- **Top-level sections gather by collection**: `/notes/` lists every page in `notes`,
  wherever filed; home and sidebar counts match. **Sub-folders don't** — they list only
  what's filed under them.
- **A collection with no folder** (`collections: [recipe]`) is still a chip and a filter
  value; it just has no section page.

```yaml
# posts/2026-09-28-hello.md — listed in Posts and Blog, filterable as either
collections: [blog]
```

## Front Matter Reference

All front matter optional; sync derives the rest.

| Key                     | Purpose                    | Notes                                                                           |
| ----------------------- | -------------------------- | ------------------------------------------------------------------------------- |
| `title`                 | Page title                 | If unset: an H1 on the first non-blank line (removed from body), else the file name (an `_index.md`: its folder's). `params.titleFromHeading = false` skips the H1 step; the H1 stays in the body |
| `collections`           | Extra collections, value or list | Page also lists under those top-level sections and collection filters. See [Collections](#collections) |
| `tags`                  | Inline or block list       | Merged with `#hashtags`; deduplicated                                           |
| `created`               | ISO 8601 datetime          | Derived from path, git, mtime, or build time if not set                         |
| `updated`               | ISO 8601 datetime          | Derived from git, mtime, or build time if not set; never earlier than `created` |
| `author`, `source_repo` | Metadata                   | Passed through; not filterable                                                  |
| `bookmark`, `bookmarks` | URL or list                | Adds collection `bookmarks`, wherever the page lives. See [Collections](#collections) |
| `facebook`, `mastodon`, … | URL or list            | Where else it was posted; keys and labels in `[params.sources]`. Shown as "Posted on" links |
| `fork_remote`, `fork_repo`, `fork_path`, `fork_branch`, `fork_commit`, `fork_blob` | Fork provenance (PublicTxt spec §5.2) | Make a page a fork: its source, source rating and link to the original |
| `mirror_remote`, `mirror_repo`, `mirror_path`, `mirror_branch`, `mirror_commit`, `mirror_blob` | Mirror provenance (PublicTxt spec §5.5) | Make a page a mirror, unless it is a fork: the same, shown as "From" |
| `url`, `local`, `rating` in `follows/` | A follow (PublicTxt spec §3.2) | `url` is written out as `follow_url`, canonical: Hugo reads `url` as the page's address |
| `source_name`, `source_rating`, `source_follow`, `source_original`, `source_kind`, `follow_name`, `follow_url` | A fork's or a mirror's source (`source_kind: mirror` on a mirror); a follow's repo | Written by sync; dropped if authored |
| `source_path`           | Path in the source repo    | Written by sync; with `params.editURL`, drives the footer "Edit this page" link |

Other keys pass through unchanged. `hugo.toml` maps `created` → `.Date`, `updated` → `.Lastmod`.

## Deployment

Build outputs to `public/`. GitHub Actions workflow (`deploy/publish-to-github-pages.yml`) checks out the content repo, runs the build pipeline, and deploys via `actions/deploy-pages`.

Per-site settings: `HUGO_*` env vars in the workflow, or the content repo's `settings/site.toml`, merged over `hugo.toml` (env vars win).

## Not built, and what would prompt it (TBD)

- `[[wikilink]]` conversion at sync — a repo in hand where legacy wikilinks bite.
- `author` / `source_repo` filters — carried already, but one value per site, so the facets would hide; multi-repo or multi-author content. (`source_repo` is where a page is published from; where its text came from is its source, above.)
- Multi-repo fan-in — aggregation is upstream PublicTxt's job; needs a defined aggregate content model.
- Backlinks ("Linked from") — link graph is cheap at sync; keep edges out of the list index (per-page fetch), a count in it.
- Link weights, trust tiers beyond this author's own rating of a follow (the source rating) — no data source yet.
- Related pages by tag co-occurrence — the site index already holds the data.
- Remove Category Sidebar, linking to search until we have Category Pages

## Issues

- see `docs/wiki/issues.md`
