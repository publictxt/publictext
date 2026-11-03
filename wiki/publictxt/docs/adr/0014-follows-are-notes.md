# 0014. Follows are notes, one per repo, in `follows/`

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

- ADR-0005 makes the follow list public text, under `follows` in `publictxt.yaml`. obsidian-publictxt keeps it in private plugin data for now (ADR-0011).
- A YAML list says which repos, not why. A readable page per followed repo (why I follow it, what to read there) makes the follow list worth reading.
- Keeping that page and a `follows` entry in step means two copies of one list. Writing `follows` from the pages would be derived data on disk (ADR-0003).
- Forks already record themselves in their own front matter and are found by scanning (ADR-0012). Follows can work the same way.

## Decision

- A follow is a document in `follows/` whose front matter has a valid `url` (spec §3.2). One document per followed repo; the body is free prose about it.
- Its front matter holds the fields `follows` entries had: `url`, `branch`, `include`. Readers MUST ignore other keys there, so a tool can keep its own settings beside them.
- The follow list is derived by scanning `follows/`, never stored.
- `follows/` is a top-level folder, so follows are content: published as pages, in the collection `follows` (spec §4.2). Not `settings/`, which PublicTxt-Pages treats as not content.
- `follows` leaves the manifest.
- The folder, not a key prefix, makes a document a follow, so `url`, `branch` and `include` mean this only in `follows/`.

## Consequences

- + One source of truth, readable in Obsidian and on the published site.
- + Obsidian Properties edits each follow: no list of objects in front matter.
- + Same shape as forks (ADR-0012): the record is the note, the list is a scan.
- − Moving a follow out of `follows/` unfollows the repo. Location carries meaning, unlike `fork_*`.
- − Readers scan a folder instead of reading one key. They already scan every document.
- − A breaking change to the manifest. Nothing reads `follows` yet.
- + The follow list is public and readable on the published site, as ADR-0005 wants.
- ! A tool's private caches (obsidian-publictxt's clones) go in a hidden folder such as `.remotes/`, kept out of the repo, never beside `follows/`.
