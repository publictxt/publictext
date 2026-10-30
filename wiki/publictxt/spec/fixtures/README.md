# Conformance fixtures

Each case is a directory:

```
NNN-name/
  repo/           # a PublicTxt repository (no .git needed)
  expected.json   # Reader output, spec §10
```

A Reader passes a case when its output for `repo/` equals `expected.json`. Compare as parsed JSON: key order is ignored, array order matters.

| Case | Covers |
|---|---|
| 001-minimal | Default layout, H1 and filename titles, relative link |
| 002-tags | Tag sources, inline grammar edge cases, raw/match, de-duplication |
| 003-blog | Date from filename, optional sequence, datetime and date-only front matter, no time zone conversion, invalid date, date diagnostics |
| 004-bookmark | `bookmark` attribute on any type, `resource_id`, URL normalization v0, type override, mismatch, missing attribute, `url` is not a bookmark |
| 005-existing-vault | Custom `types`, longest prefix, `ignore`, default `page` type, link resolution |

Rules for adding cases:

- Keep to one concern per case where possible.
- Every change to a [P] rule changes a fixture in the same commit.
- Files are UTF-8, NFC, LF.
