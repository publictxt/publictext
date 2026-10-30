# 0010. Bookmark is an attribute any document can carry

- **Status:** Accepted (any document can be a bookmark). Proposed (the `bookmark` key name).
- **Date:** 2026-10-13

## Context

- Spec 0.1 drafts made a bookmark a type: files under `metaweb/`, or `type: bookmark`.
- Many pages are already about one resource: a wiki page on a book, a note on a paper, a blog post reviewing a tool. Under a type-only model, each would need a duplicate `metaweb/` file to show up in CuratedCommons.
- The earlier draft used front matter `url` for the resource. Hugo treats `url` as a page path override, so publictxt-pages would publish bookmarks at broken paths.

## Decision

- Any document with a `bookmark` attribute is a bookmark and keeps its own type.
- The attribute is front matter `bookmark: <url>`. `url` is not reserved.
- Type `bookmark` remains for `metaweb/` files that exist only to bookmark a resource, and those files must carry the attribute.
- `resource_id` is computed for every document with the attribute.

## Consequences

- + CuratedCommons aggregates every bookmarked page, not only `metaweb/`, with no duplicate files.
- + Supports ADR-0007: bookmarking is a facet across types, which a separate MetaWeb module could not own.
- − The same resource can appear in several documents in one repo. Views group by `resource_id`. Whether that should warn is open (spec §13).
- − Any existing content or the publictxt-pages scaffold that uses `url` for bookmarks must be renamed to `bookmark`.
