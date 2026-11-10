# PublicTxt Format Specification

**Version:** 0.1.0-draft · **Date:** 2026-11-03

Defines how a Git repository stores PublicTxt content so that independent tools can read, validate, aggregate and publish it. Rendering, UI, search and indexing are out of scope.

Every rule carries a status marker:

- **[D]** decided
- **[P]** proposed, needs sign-off
- **[O]** open, listed in §13

MUST, SHOULD and MAY are used as in RFC 2119.

The layout and page rules follow [PublicTxt-Pages](https://github.com/publictxt/publictxt-pages/blob/main/docs/SPEC.md), the static site that publishes PublicTxt repos (ADR-0013). Where this spec and Pages disagree, §13 lists it.

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
- [P] Repositories MUST NOT commit derived data: indexes, tag pages, search data, normalized tags (ADR-0003). One open exception: §8.
- [P] Content is published on the default branch. Other branches are for drafts and pull requests, not distribution (ADR-0005).
- [P] Documents are `*.md` files in UTF-8. Writers SHOULD use LF line endings and Unicode NFC. Readers MUST accept CRLF.
- [P] Every other file is an attachment: it can be a link target but is not a document.
- [P] Readers skip:
  - the folders `.git/`, `.obsidian/`, `.trash/`, `_site/` and `node_modules/`, at any depth;
  - the files `CNAME` and `.gitignore`, at any depth;
  - every path listed under `ignore` in the manifest.
- [P] `README.md`, `LICENSE.md` and `CONTRIBUTING.md` are not skipped by default: they are documents like any other, and `LICENSE` is an attachment. A Reader MAY let a repo name further file names to skip (PublicTxt-Pages: `params.skipFiles`).

## 3. Manifest: `publictxt.yaml`

[P] A YAML file at the repo root. A Reader MUST accept a repo without one and emit `no-manifest`.

```yaml
publictxt: "0.1"            # required: spec version the repo targets
author:
  name: Jaysen              # required: display name
ignore:                     # optional: path prefixes Readers skip
  - .obsidian/
  - templates/
aliases: aliases.yaml       # optional: this author's tag alias file (§7.4)
```

- `ignore` entries are repo-relative folder prefixes ending in `/`.
- [P] `types` (folder → type) is gone: collections come from folder names (§4).
- Readers MUST ignore unknown keys.
- [P] A repo's identity is its remote URL, not a manifest field, in canonical form (§3.1).
- [P] `follows` is gone: follows are documents (§3.2, ADR-0014).

### 3.1 Canonical repo URL

[P] v0, adopted from obsidian-publictxt-remotes, which needs a stable key for follows and forks:

- Form: `https://<host>/<owner>/<repo>`.
- Lower-case the scheme and host. Owner and repo keep their case: hosts differ on case sensitivity.
- Drop a `.git` suffix, a trailing `/`, the query and the fragment.
- `git@<host>:<owner>/<repo>.git` becomes the HTTPS form.
- Anything else is invalid, including `file:` URLs. A follow of a repo with no public URL has no `url` at all (§3.2).
- [O] Hosts with nested groups (GitLab subgroups), other ports and self-hosted paths.

### 3.2 Follows

[P] The repos this author follows. Each is a document in `follows/`, at any depth, with front matter (ADR-0014):

```yaml
---
url: https://github.com/example/notes   # required: canonical repo URL (§3.1)
branch: main                            # optional; default: the remote's default branch
include: [blog/]                        # optional folder prefixes; default: the whole repo
---
Why I follow it, and what to read there.
```

- A document in `follows/` is a follow when `url` is a valid repo URL. Readers canonicalize it (§3.1). Without one, the Reader emits `follow-missing-url` and it is a plain document.
- A follow's name is its file name without `.md`: `follows/ann-notes.md` is the follow `ann-notes`. Forks name the follow they came through by it (`fork_remote`, §5.2). Names are unique in a repo, compared without case.
- [P] A document in `follows/` with `local: true` and no `url` is a tool's follow of a repo with no public URL, such as another vault on the author's computer (ADR-0015). It holds no path. To Readers it is a plain document, without `follow-missing-url`.
- `branch` names the branch to follow; when absent or blank, the remote's default branch.
- `include` lists repo-relative folder prefixes ending in `/`, like `ignore`; when absent or empty, the whole repo. It narrows what a reader shows, not what it fetches: the whole repo is still fetched (ADR-0005). It is the first piece of the filter grammar; tag filters stay open (§13).
- Two follows of one canonical URL: the first by path (byte order) counts; the Reader emits `duplicate-follow` on the others.
- Readers MUST ignore other keys in a follow: tools keep their own settings there.
- Follows are documents like any other: in the collection `follows` (§4.2), and published.
- [O] Whether the Document model lists follows under `repo` (§13).

## 4. Layout and collections

### 4.1 Layout

[P] Any folder layout is valid: an existing Obsidian vault needs no restructuring. This layout is the convention, and what PublicTxt-Pages builds sections from:

| Path | Holds |
|---|---|
| `_index.md` | The home page |
| `wiki/` | Reference pages, read by place |
| `blog/` | Dated posts; the date usually comes from the path (§5.1) |
| `posts/` | Longer pieces |
| `notes/` | Short notes |
| `bookmarks/sites/<domain>/…/<file>.md` | One file per web resource, with a `bookmark` URL (§8) |
| `bookmarks/wiki/` | Pages about bookmarks; no URL needed |
| `media/` | Shared attachments |
| `settings/site.toml` | PublicTxt-Pages site settings; not content |
| `follows/` | One document per followed repo (§3.2) |

Blog paths, each dated by §5.1:

```txt
blog/2023/12/17/post.md          day folders
blog/2024/09/20240922-title.md   YYYYMMDD file name prefix, month folders
blog/20260509-title.md           YYYYMMDD file name prefix, no folders
```

- [P] `_index.md` is a folder's own page, at any depth. It is a document like any other.
- [P] Attachments sit beside the note that uses them, or in a top-level folder such as `media/`.
- [O] `community/` (profiles, groups, discussion) is deferred.

### 4.2 Collections

[P] A document is in one or more collections. They replace types (ADR-0013).

| Source | Collection it adds |
|---|---|
| The top-level folder the document is filed in | That folder's name |
| A document at the repo root | `page` |
| Front matter `collections` (one string or a list) | Each value |
| A `bookmark` or `bookmarks` URL (§8) | `bookmarks` |

- Collections are lower-cased: `Notes` and `notes` are one collection.
- Order: the folder's (or `page`), then `collections` values as written, then `bookmarks`. Duplicates are dropped; the first stays.
- Collections add, never replace: the folder's collection always stays.
- A collection needs no folder: `collections: [recipe]` is valid on its own.

## 5. Front matter

[P] Optional. If a file's first line is exactly `---`, the YAML block runs to the next line that is exactly `---`. This matches Obsidian Properties. If the block does not parse to a mapping, the Reader emits `invalid-frontmatter` and treats the file as having no front matter.

Reserved keys, all optional:

| Key | Value | Meaning |
|---|---|---|
| `title` | string | Document title (§6) |
| `collections` | string, or list of strings | Extra collections (§4.2) |
| `tags` | list of strings, or one string | Tags (§7) |
| `created` | ISO 8601 datetime or date | When the document was written (§5.1). Legacy: `date` |
| `updated` | ISO 8601 datetime or date | When it last changed (§5.1). Legacy: `lastmod` |
| `publish` | `off`, `false`, `no` or `0` | Not published (§5.3) |
| `bookmark`, `bookmarks` | absolute URL, or list of them | Makes the document a bookmark of each resource (§8) |
| `resource_id` | string | See §8 |
| `aliases` | list of strings | Obsidian note aliases (not tag aliases) |
| `fork_*` | see §5.2 | Fork provenance |

- Readers preserve unknown keys and pass them through. PublicTxt-Pages also reads `categories`, `rating`, `author`, `source_repo` and post source keys (`mastodon`, `substack`, …); to this spec they are unknown keys.
- [P] `type` is not reserved: collections replace it (§4.2).
- [P] `url` is not reserved. Hugo uses front matter `url` to override a page's output path, so a bookmark stored as `url` would break the static site. Readers treat `url` like any unknown key.
- [O] `author` and `source_repo` can be derived from the manifest and the repo URL. The Hugo interface currently carries them in front matter. Decide whether they are written to disk (derived data, ADR-0003) or injected at publish time.

### 5.1 Dates

[P] Every document has a `created` and an `updated`, resolved by a ladder. Each rung that applies wins over the ones below it. The Reader records the rung for `created` as `created_source`.

`created`:

1. `front-matter`: `created`, else the legacy `date`
2. `path`: a date in the file name, else in the folders (below)
3. `git`: the first commit that touched the file
4. `mtime`, then `build`: the file system and the build time

`updated`: front matter `updated`, else the legacy `lastmod`; then the last commit, the file's mtime, the build time. Consumers keep `updated` no earlier than `created`.

- [P] Rungs 3 and 4 depend on the checkout, not the files: a shallow clone collapses rung 3. The Document model (§10) holds rungs 1 and 2 only, and is null where neither applies. Consumers fill in the rest.
- [P] A path date is the first of:
  - `YYYY-MM-DD` in the file name without its extension;
  - `YYYYMMDD` in the file name without its extension;
  - `YYYY/MM/DD/` in the folders.

  Either file name form must not touch another digit, so a longer run (an ISBN, an id) is not a date. A form that isn't a real calendar date is skipped. Month folders alone (`2024/09/`) are not a date. The path date applies in any folder, not only `blog/`, and is the date-only form `YYYY-MM-DD`.
- [D] Front matter dates are ISO 8601. Writers SHOULD include the time and UTC offset: `2026-10-13T00:36:00+02:00`.
- [D] A date-only value (`2026-10-13`) is valid and produces no diagnostic.
- [P] Accepted forms: `YYYY-MM-DD`, optionally followed by `T` or a space and `hh:mm`, then optional `:ss` with an optional fraction, then optional `Z` or `±hh:mm`. This covers the values Obsidian's date and date-and-time properties write.
- [P] Readers MUST use the value as written and MUST NOT convert time zones. YAML libraries disagree on timestamps: YAML 1.1 resolves them to typed values, while the YAML 1.2 core schema leaves them as strings. Relying on typed parsing would make Readers disagree.
- [P] A value that doesn't match these forms, or isn't a real calendar date and time (`2026-02-30`), emits `invalid-date` and is treated as absent: the next rung applies.

### 5.2 Fork provenance

[P] A fork is a document copied from another repo. It records where it came from in its own front matter, flat so Obsidian Properties can show and edit it (ADR-0012).

| Key | Value |
|---|---|
| `fork_remote` | Name of the follow (§3.2) in this repo it was forked through |
| `fork_repo` | Canonical repo URL (§3.1) of the source; absent when the follow has none (`local: true`) |
| `fork_path` | Repo-relative path of the file forked |
| `fork_branch` | Branch it was forked from |
| `fork_commit` | Source head commit SHA at fork time |
| `fork_blob` | Git blob SHA of the source file at fork time: the change marker and merge base |
| `fork_date` | ISO 8601 datetime of the fork, with offset |

- [P] A document is a fork when `fork_path` and `fork_blob` are valid and it has a valid `fork_repo` or `fork_remote`. The others are optional when reading.
- [P] A Writer writes `fork_remote`, and `fork_repo` whenever the source has a URL: the URL is what lets anyone reading a published fork find the original, which a name alone can't.
- [P] A Writer that forks keeps the source's front matter, sets its `fork_*` keys, and removes any other `fork_*` key, so a fork of a fork never keeps the upstream's provenance.
- [P] These are authored facts, not derived data: they can't be recomputed from the repo (ADR-0003).
- [O] Whether the Document model (§10) exposes fork provenance. In 0.1 Readers pass `fork_*` through like any unknown key.

### 5.3 Publishing

- [P] `publish: off` (or `false`, `no`, `0`, case-insensitive) keeps a document off published sites. It is hidden, not private: the file is still public in the repo.
- [P] Readers still read unpublished documents and set `published: false` (§10). Whether followers aggregate them is up to the consumer.

## 6. Title

[P] First match wins:

1. Front matter `title`
2. An ATX H1 (`# Heading`) if it is the first non-empty line in the body.
3. For `_index.md`, its folder's name with `-` replaced by spaces (at the repo root, `index`). Otherwise the file name without `.md`, with `-` and `_` replaced by spaces and trimmed. A date prefix stays: `20260509-title.md` is titled `20260509 title`.

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

## 8. Bookmarks

A bookmark is a document about a web resource. Being a bookmark is an attribute, not a folder (ADR-0010).

- [D] Any document with a `bookmark` attribute is a bookmark, wherever it lives. A wiki page about a book stays in `wiki` and joins `bookmarks` (§4.2).
- [P] The attribute is front matter `bookmark` or `bookmarks`: an absolute `http` or `https` URL, or a list of them. Both keys may be present; URLs are read `bookmark` first, then `bookmarks`, duplicates dropped.
- [P] A document under `bookmarks/`, except under `bookmarks/wiki/` and except `_index.md`, without a URL emits `bookmark-missing-url`. It is still in the `bookmarks` collection by folder.
- [P] Several documents in one repo may bookmark the same resource, e.g. a `bookmarks/sites/` entry and a wiki page. Views group them by `resource_id` and combine their tags.

### resource_id

- [D] `resource_id` identifies the resource across repos. It is a hash of the normalized URL.
- [P] `resource_id = "sha256:" + lowercase hex SHA-256 of the UTF-8 bytes of normalize(url)`.
- [P] `normalize` v0 is deliberately minimal:
  - lowercase the scheme and host;
  - drop the default port (`:80` for http, `:443` for https);
  - drop the fragment;
  - an empty path becomes `/`;
  - change nothing else: no query reordering, no trailing-slash or `www.` changes.
- [O] Full normalization and near-miss handling are deferred.
- [P] Readers compute `resource_id` for every bookmark URL. If front matter holds a `resource_id`, the document has exactly one URL and the two differ, the Validator emits `resource-id-mismatch`.
- [O] A stored `resource_id` is derived data (ADR-0003). Changing `normalize` would make every stored value stale. The options are (a) never store it and compute it when reading, or (b) store it with a normalization version prefix. Recommendation: (a), unless a consumer needs it on disk.
- [O] The mapping from URL to a `bookmarks/sites/` path is undefined. Readers MUST NOT validate it in 0.1.

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

This is the Reader's output and the shape of every fixture's `expected.json`. Documents are sorted by `path` in byte order. Collections, bookmarks, tags and links keep their order.

```json
{
  "repo": { "spec": "0.1", "author": "Jaysen", "diagnostics": [] },
  "documents": [
    {
      "path": "notes/quick-link.md",
      "collections": ["notes", "reading", "bookmarks"],
      "title": "Quick link",
      "created": "2026-10-13T00:36:00+02:00",
      "created_source": "front-matter",
      "updated": null,
      "published": true,
      "bookmarks": [{ "url": "http://Example.com:8080/a/b", "resource_id": "sha256:…" }],
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

- `collections` follows §4.2 and is never empty.
- `created` is the front matter value exactly as written, else the path date (`YYYY-MM-DD`), else null. `created_source` is `front-matter`, `path` or null (§5.1).
- `updated` is the front matter value exactly as written, else null.
- `published` is false only for `publish: off` and its synonyms (§5.3).
- `bookmarks` lists each URL as written, with its computed `resource_id`; empty when the document is not a bookmark.
- `tags[].source` is `frontmatter` or `inline`. `links[].syntax` is `markdown` or `wikilink`.
- Each diagnostic is `{ "level": "warning" | "error", "code": string, "subject": string | null }`.

## 11. Diagnostics

| Code | Level | Scope | `subject` |
|---|---|---|---|
| `no-manifest` | warning | repo | null |
| `unsupported-spec` | warning | repo | the manifest's version |
| `invalid-frontmatter` | error | document | null |
| `invalid-date` | warning | document | the value as written |
| `bookmark-missing-url` | warning | document | null |
| `resource-id-mismatch` | warning | document | the stored value |
| `unresolved-link` | warning | document | the link `target` |
| `follow-missing-url` | warning | document | null |
| `duplicate-follow` | warning | document | the canonical URL |

## 12. Versioning

- [P] The spec follows SemVer. Any 0.x minor version may break compatibility.
- [P] The manifest's `publictxt` key declares the version a repo targets. A Reader SHOULD emit `unsupported-spec` when it is newer than the Reader supports.

## 13. Open questions

1. Filter grammar for follows beyond `include` folder prefixes, e.g. tags (§3.2)
2. Canonical repo URL beyond v0: nested groups, ports, self-hosted paths (§3.1)
3. `author` and `source_repo`: stored on disk or injected at publish (§5)
4. Mapping from URL to a `bookmarks/sites/` path (§8)
5. Full URL normalization and near-miss handling (§8)
6. Whether `resource_id` is stored on disk (§8)
7. Bare URLs as links (§9)
8. Parity with Obsidian link resolution (§9)
9. `community/`: groups and shared repos (§4.1)
10. Dataview inline fields (`key:: value`, e.g. `bookmark:: <url>`). The README requires Dataview compatibility; 0.1 does not parse them.
11. Whether two documents in one repo bookmarking the same resource should warn (§8)
12. Whether the Document model exposes fork provenance (§5.2)
13. Detecting local edits to a fork: needs a hash of the body without the `fork_*` keys (§5.2)
14. Where folders are left out: manifest `ignore` here, `params.excludeFolders` in PublicTxt-Pages' `settings/site.toml`. One place would be better (§3)
15. Differences from PublicTxt-Pages still to settle (ADR-0013):
    - Tags: Pages reads ASCII letters only and lower-cases at build; this spec allows Unicode letters and keeps `raw` (§7). Pages also tags a page with a nested tag's parents (`#dev/app` → `dev`).
    - Invalid dates: Pages passes an invalid `created` through; this spec drops it and moves to the next rung (§5.1).
    - Links: Pages shows `[[wikilinks]]` as plain text; this spec extracts and resolves them (§9).
16. Whether the Document model lists follows under `repo` (§3.2)

## Appendix A. Changes from the README layout

- `indexes/` and `tags/` folders removed: derived data (ADR-0003).
- Topic branches removed (ADR-0005).
- Bookmarks are one file per resource under `bookmarks/sites/<domain>/`, replacing `metaweb/` and its sub-folders (`sites`, `bookmarks`, `notes`, `annotations`) (ADR-0013).
- Any document can be a bookmark through the `bookmark` attribute, wherever it lives [D] (ADR-0010).
- Blog posts keep all three PublicTxt-Pages path forms, day folders included (ADR-0013).
- `community/` deferred.
- W3C Web Annotation moved from storage model to a possible export format ([IDEAS.md](../IDEAS.md)) [D].

## Appendix B. Changes from the 2026-10-13 draft (ADR-0013)

- Types (`type`, `type_source`, manifest `types`) replaced by collections (§4.2).
- `date` replaced by `created` and `updated`, with `date` and `lastmod` read as legacy keys (§5.1). Path dates apply in any folder; the blog sequence `<n>` is gone.
- `metaweb/` replaced by `bookmarks/sites/`. `bookmarks` lists are allowed; the model's `bookmark` and `resource_id` became `bookmarks[]`.
- `bookmark-missing-url` is a warning, scoped to `bookmarks/` outside `bookmarks/wiki/`. `blog-missing-date` and `blog-date-mismatch` removed.
- Titles keep date prefixes and replace `-` and `_` with spaces; `_index.md` takes its folder's name (§6).
- `publish` reserved; `published` added to the model (§5.3).
- Editor folders, `CNAME` and `.gitignore` skipped; READMEs, licences and contributing files are not (§2).
