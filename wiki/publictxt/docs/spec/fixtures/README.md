# Conformance fixtures

Each case is a directory:

```
NNN-name/
  repo/           # a PublicTxt repository (no .git needed)
  expected.json   # Reader output, spec §10
```

A Reader passes a case when its output for `repo/` equals `expected.json`. Compare as parsed JSON: key order is ignored, array order matters.

Fixtures have no `.git`, so `created` and `updated` cover the front matter and path rungs only (spec §5.1).

| Case | Covers |
|---|---|
| 001-minimal | Folder collection, H1 as first non-empty line (a later H1 or one inside code is ignored), file name and `_index.md` titles, `publish: off`, README as a document, relative link |
| 002-tags | Tag sources, inline grammar edge cases, raw/match, de-duplication |
| 003-dates | The three blog path forms, path dates in any folder, digit guard, `created`/`updated` and legacy `date`/`lastmod`, datetime and date-only values, no time zone conversion, invalid date |
| 004-bookmark | `bookmark` on any document, `bookmarks` lists, `bookmarks/sites/` and `bookmarks/wiki/`, `collections`, `resource_id`, URL normalization v0, mismatch, missing URL, `url` is not a bookmark |
| 005-existing-vault | Vault folders as collections, root `page`, `ignore`, link resolution |

Rules for adding cases:

- Keep to one concern per case where possible.
- Every change to a [P] rule changes a fixture in the same commit.
- Files are UTF-8, NFC, LF.
