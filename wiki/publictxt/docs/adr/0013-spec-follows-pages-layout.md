# 0013. The spec follows the PublicTxt-Pages layout and page rules

- **Status:** Proposed
- **Date:** 2026-11-03
- **Supersedes:** the `type` model of ADR-0007; the `metaweb/` bookmark type of ADR-0010

## Context

- Two layouts existed. The 0.1 draft spec had types (`wiki`, `blog`, `note`, `bookmark`, `page`), `metaweb/`, `blog/<yyyy>/<mm>/<yyyymmdd>[-<n>]-<slug>.md` and one `date`.
- PublicTxt-Pages, this repo and obsidian-publictxt-facets use another: collections from top-level folders, `bookmarks/sites/`, three blog path forms, `created`/`updated` from a date ladder, `publish: off`.
- Pages is released and in use; nothing implements the draft spec's layout. ADR-0002 makes the spec the product, so a spec nobody follows defines nothing.

## Decision

- The spec adopts the Pages layout and page rules: collections, dates, titles, bookmarks, publishing and skipped files (spec §2, §4–§6, §8).
- Collections replace types. The model's `type` and `type_source` become `collections`; manifest `types` is dropped.
- The Document model carries only what the files determine. `created` stops at the path rung; git, mtime and build time are left to consumers, since they depend on the checkout.
- Where the spec is stricter than Pages (Unicode tags, `raw` tags, H1 outside code, invalid dates dropped), it keeps its rule for now. Spec §13 lists each difference; settling one changes Pages or the spec, through an ADR.
- Wikilinks are deferred, as in Pages: links are relative Markdown links (spec §9).
- From here on, a change to the layout or page rules goes into the spec first and Pages follows.

## Consequences

- + One layout. This repo, Pages and the Facets plugin already conform to most of it.
- + An existing vault needs no `types` mapping: its folders are its collections.
- + Several bookmarks per document, which the draft left open.
- − Breaking change to the draft model and every fixture. Nothing implements the draft yet, so the cost is the fixtures alone.
- − Reverses rules the draft marked decided [D]: blog day folders are back, and `metaweb/` is renamed `bookmarks/sites/` (still one file per resource).
- − Titles keep date prefixes (`20260509 title`), as Pages does. Stripping them would be a Pages change.
- − Folder exclusion lives in two places, manifest `ignore` and Pages' `excludeFolders` (spec §13).
