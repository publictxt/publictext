# PublicTxt Format Specification

**Version:** 0.1.0-draft · **Date:** 2026-10-13

Defines how a Git repository stores PublicTxt content so that independent tools can read, validate, aggregate and publish it. Rendering, UI, search and indexing are out of scope.

Every rule carries a status marker:

- **[D]** decided
- **[P]** proposed, needs sign-off
- **[O]** open, listed in §13

MUST, SHOULD and MAY are used as in RFC 2119.

## 1. Conformance

| Role | Requirement |
|---|---|
| Reader | Parses a repo into the Document model (§10). MUST produce the expected output for every case in `spec/fixtures/`. |
| Writer | Produces files that a conformant Reader parses without errors. |
| Validator | Reports diagnostics (§11). MUST NOT modify files. |

A tool states the role and version it conforms to, e.g. "Reader 0.1".

## 2. Repository

- [D] One author per repository. Multi-author views come from aggregating repositories (ADR-0009).
- [D] Git is the source of truth.
- [P] Repositories MUST NOT commit derived data: indexes, tag pages, search data, normalized tags (ADR-0003). One open exception: §8.2.
- [P] Content is published on the default branch. Other branches are for drafts and pull requests, not distribution (ADR-0005).
- [P] Documents are `*.md` files in UTF-8. Writers SHOULD use LF line endings and Unicode NFC. Readers MUST accept CRLF.
- [P] Every other file is an attachment: it can be a link target but is not a document.
- [P] Readers skip `.git/` and every path listed under `ignore` in the manifest.

## 3. Manifest: `publictxt.yaml`

[P] A YAML file at the repo root. A Reader MUST accept a repo without one, apply the default layout (§4) and emit `no-manifest`.

```yaml
publictxt: "0.1"            # required: spec version the repo targets
author:
  name: Jaysen              # required: display name
types:                      # optional: folder prefix → type; replaces §4 when present
  wiki/: wiki
  blog/: blog
  notes/: note
  metaweb/: bookmark
ignore:                     # optional: path prefixes Readers skip
  - .obsidian/
  - templates/
follows:                    # optional: repos this author subscribes to
  - url: https://github.com/example/notes
    filter: "tag:dotnet"    # [O] filter grammar
aliases: aliases.yaml       # optional: this author's tag alias file (§7.4)
```

- Keys in `types` are repo-relative folder prefixes ending in `/`. The longest matching prefix wins.
- Readers MUST ignore unknown keys.
- [P] A repo's identity is its remote URL, not a manifest field. [O] Canonical form of that URL.

## 4. Default layout

[D] Applies when the manifest has no `types`.

| Path | Type |
|---|---|
| `wiki/` | `wiki` |
| `blog/<yyyy>/<mm>/<yyyymmdd>[-<n>]-<slug>.md` | `blog` |
| `notes/` | `note` |
| `metaweb/<domain>/<subdomain-or-folder>/<file>.md` | `bookmark` |
| `media/` | attachments |

- [P] A `.md` file outside every mapped folder has type `page`.
- [O] `community/` (profiles, groups, discussion) is deferred.

Type resolution, first match wins:

1. [D] Front matter `type`
2. [D] Folder mapping
3. [P] `page`

The Reader records which rule applied as `type_source`: `frontmatter`, `folder` or `default`.

## 5. Front matter

[P] Optional. If a file's first line is exactly `---`, the YAML block runs to the next line that is exactly `---`. This matches Obsidian Properties. If the block does not parse to a mapping, the Reader emits `invalid-frontmatter` and treats the file as having no front matter.

Reserved keys, all optional unless §8 says otherwise:

| Key | Value | Meaning |
|---|---|---|
| `type` | string | Overrides the folder type |
| `title` | string | Document title |
| `tags` | list of strings, or one string | Tags (§7) |
| `date` | ISO 8601 datetime or date | Publication date (§5.1) |
| `bookmark` | absolute URL | Makes the document a bookmark of that resource (§8.2) |
| `resource_id` | string | See §8.2 |
| `aliases` | list of strings | Obsidian note aliases (not tag aliases) |

- Readers preserve unknown keys and pass them through.
- [P] `url` is not reserved. Hugo uses front matter `url` to override a page's output path, so a bookmark stored as `url` would break the static site. Readers treat `url` like any unknown key.
- [O] `author` and `source_repo` can be derived from the manifest and the repo URL. The Hugo interface currently carries them in front matter. Decide whether they are written to disk (derived data, ADR-0003) or injected at publish time.

### 5.1 Dates

- [D] `date` is an ISO 8601 datetime. Writers SHOULD include the time and UTC offset: `2026-10-13T00:36:00+02:00`.
- [D] A date-only value (`2026-10-13`) is valid and produces no diagnostic.
- [P] Accepted forms: `YYYY-MM-DD`, optionally followed by `T` or a space and `hh:mm`, then optional `:ss` with an optional fraction, then optional `Z` or `±hh:mm`. This covers the values Obsidian's date and date-and-time properties write.
- [P] Readers MUST use the value as written and MUST NOT convert time zones. YAML libraries disagree on timestamps: YAML 1.1 resolves them to typed values, while the YAML 1.2 core schema leaves them as strings. Relying on typed parsing would make Readers disagree.
- [P] A value that doesn't match these forms, or isn't a real calendar date and time (`2026-02-30`), emits `invalid-date` and is treated as absent.

## 6. Title

[P] First match wins:

1. Front matter `title`
2. The first ATX H1 (`# Heading`) in the body, outside code
3. The filename without `.md`. For `blog`, also without the date and sequence prefix.

## 7. Tags

### 7.1 Sources

- [D] Front matter `tags`. A leading `#` is stripped. Writers SHOULD omit it, because an unquoted `#` starts a YAML comment.
- [D] Inline hashtags in the body.

### 7.2 Inline grammar

[P] A tag is `#` followed by one or more tag characters, where:

- the `#` is at the start of a line or preceded by whitespace;
- tag characters are Unicode letters, digits, `_`, `-` and `/`;
- at least one character is not a digit (`#2026` is not a tag).

Tags are not recognized inside front matter, fenced code blocks or inline code. These rules also exclude headings (`# Title`) and URL fragments (`/page#section`).

### 7.3 Raw and match forms

- [D] `raw` is the tag as written, without `#`. It is never rewritten on disk.
- [D] `match` is computed when reading and never written to disk.
- [P] `match` = Unicode NFC, then lowercase. The `/` hierarchy is kept.
- [P] Tags are de-duplicated by `match`: front matter tags first, then inline tags in reading order. The first occurrence's `raw` is kept.

### 7.4 Tag aliases

[D] Vocabulary is reconciled at view time with the reader's own alias file. Tags are never normalized globally (ADR-0006).

[P] File format, keyed by `match`:

```yaml
dotnet: [dot-net, net-core]
```

Aliases apply in views only. They are not part of the Document model.

## 8. Type and attribute rules

### 8.1 blog

- [D] Path: `blog/<yyyy>/<mm>/<yyyymmdd>[-<n>]-<slug>.md`, e.g. `20260920-1-title.md`.
- [P] `<n>`, the per-day sequence, is optional. The first all-digit segment after the date is always read as `<n>`, so a slug that starts with a number (`2026-plans`) needs an explicit `<n>`.
- [P] `date` = front matter `date`, else the filename prefix. If neither exists, `date` is null and the Validator emits `blog-missing-date`.
- [P] The Validator emits `blog-date-mismatch` when the `<yyyy>/<mm>` folders disagree with `date`. The check uses the calendar date as written, without time zone conversion.

### 8.2 Bookmarks

A bookmark is a document about one web resource. Being a bookmark is an attribute, not a type.

- [D] Any document with a `bookmark` attribute is a bookmark, whatever its type. A wiki page about a book keeps type `wiki` and is also a bookmark (ADR-0010).
- [P] The attribute is front matter `bookmark: <url>`, an absolute `http` or `https` URL, one per document.
- [D] Type `bookmark` is for documents that exist only to bookmark a resource: one file per resource, at `metaweb/<domain>/<subdomain-or-folder>/<file>.md`.
- [P] A document of type `bookmark` without the attribute is an error: `bookmark-missing-url`.
- [O] The exact mapping from URL to `metaweb/` path is undefined. Readers MUST NOT validate it in 0.1.
- [P] Several documents in one repo may bookmark the same resource, e.g. a `metaweb/` entry and a wiki page. Views group them by `resource_id` and combine their tags.

#### resource_id

- [D] `resource_id` identifies the resource across repos. It is a hash of the normalized URL.
- [P] `resource_id = "sha256:" + lowercase hex SHA-256 of the UTF-8 bytes of normalize(bookmark)`.
- [P] `normalize` v0 is deliberately minimal:
  - lowercase the scheme and host;
  - drop the default port (`:80` for http, `:443` for https);
  - drop the fragment;
  - an empty path becomes `/`;
  - change nothing else: no query reordering, no trailing-slash or `www.` changes.
- [O] Full normalization and near-miss handling are deferred.
- [P] Readers compute `resource_id` for every bookmark. If front matter also holds one and it differs, the Validator emits `resource-id-mismatch`.
- [O] A stored `resource_id` is derived data (ADR-0003). Changing `normalize` would make every stored value stale. The options are (a) never store it and compute it when reading, or (b) store it with a normalization version prefix. Recommendation: (a), unless a consumer needs it on disk.

### 8.3 wiki, note, page

No extra rules.

## 9. Links

[P] Readers extract explicit links outside code, in reading order:

- Markdown links `[text](target)` and embeds `![alt](target)`
- Wikilinks `[[target]]`, `[[target|label]]`, `[[target#heading]]` and embeds `![[target]]`

Resolution:

- [P] A target with a URI scheme (`https:`, `mailto:`) is external. Its `target` is the whole URL as written, `fragment` is null, and it is not resolved.
- [P] For internal links, `target` is as written, minus any `#fragment` and wikilink `|label`. It is not percent-decoded. Text after `#` becomes `fragment`.
- [P] Markdown targets are percent-decoded and resolved relative to the linking file's folder. A leading `/` means the repo root.
- [P] A wikilink target without an extension gets `.md`. A target containing `/` is a repo-relative path. Any other target matches by filename, case-insensitively, among files that are not ignored. Exactly one match resolves; zero or several leave the link unresolved.
- [P] An empty target (`[[#heading]]`) resolves to the linking file.
- [P] An unresolved internal link produces `unresolved-link`.
- [O] Bare URLs are not links in 0.1. Revisit for bookmarking.
- [O] Full parity with Obsidian's link resolution (shortest-path rules) is not a goal for 0.1.

## 10. Document model

This is the Reader's output and the shape of every fixture's `expected.json`. Documents are sorted by `path` in byte order. Tags and links keep reading order.

```json
{
  "repo": { "spec": "0.1", "author": "Jaysen", "diagnostics": [] },
  "documents": [
    {
      "path": "notes/quick-link.md",
      "type": "bookmark",
      "type_source": "frontmatter",
      "title": "Quick link",
      "date": null,
      "bookmark": "http://Example.com:8080/a/b",
      "resource_id": "sha256:…",
      "tags": [{ "raw": "Reading", "match": "reading", "source": "frontmatter" }],
      "links": [{
        "syntax": "markdown", "embed": false, "external": false,
        "target": "../wiki/home.md", "fragment": null, "resolved": "wiki/home.md"
      }],
      "diagnostics": []
    }
  ]
}
```

- `bookmark` is the front matter `bookmark` or null, for any type.
- `resource_id` is computed whenever `bookmark` is set, and is null otherwise.
- `date` is the front matter value exactly as written (datetime or date), else the filename date (`YYYY-MM-DD`) for blogs, else null.
- `tags[].source` is `frontmatter` or `inline`. `links[].syntax` is `markdown` or `wikilink`.
- Each diagnostic is `{ "level": "warning" | "error", "code": string, "subject": string | null }`.

## 11. Diagnostics

| Code | Level | Scope | `subject` |
|---|---|---|---|
| `no-manifest` | warning | repo | null |
| `unsupported-spec` | warning | repo | the manifest's version |
| `invalid-frontmatter` | error | document | null |
| `invalid-date` | warning | document | the value as written |
| `bookmark-missing-url` | error | document | null |
| `resource-id-mismatch` | warning | document | the stored value |
| `blog-missing-date` | warning | document | null |
| `blog-date-mismatch` | warning | document | the folder `yyyy/mm` |
| `unresolved-link` | warning | document | the link `target` |

## 12. Versioning

- [P] The spec follows SemVer. Any 0.x minor version may break compatibility.
- [P] The manifest's `publictxt` key declares the version a repo targets. A Reader SHOULD emit `unsupported-spec` when it is newer than the Reader supports.

## 13. Open questions

1. Filter grammar for `follows` (§3)
2. Canonical form of a repo's remote URL (§3)
3. `author` and `source_repo`: stored on disk or injected at publish (§5)
4. Mapping from URL to `metaweb/` path (§8.2)
5. Full URL normalization and near-miss handling (§8.2)
6. Whether `resource_id` is stored on disk (§8.2)
7. Bare URLs as links (§9)
8. Parity with Obsidian link resolution (§9)
9. `community/`: groups and shared repos (§4)
10. Dataview inline fields (`key:: value`, e.g. `bookmark:: <url>`). The README requires Dataview compatibility; 0.1 does not parse them.
11. Several resources in one document, i.e. `bookmark` as a list (§8.2)
12. Whether two documents in one repo bookmarking the same resource should warn (§8.2)

## Appendix A. Changes from the README layout

- `indexes/` and `tags/` folders removed: derived data (ADR-0003).
- Topic branches removed (ADR-0005).
- `metaweb/` sub-folders (`sites`, `bookmarks`, `notes`, `annotations`) collapsed into one file per resource [D].
- Any document can be a bookmark through the `bookmark` attribute, not only files under `metaweb/` [D] (ADR-0010).
- Blog day folders replaced by `blog/<yyyy>/<mm>/` with date-prefixed filenames [D].
- `community/` deferred.
- W3C Web Annotation moved from storage model to a possible export format (IDEAS.md) [D].
