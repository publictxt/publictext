# PublicTxt

Plain Markdown in Git as personal and public knowledge storage - published free, readable by any tool, and (next) aggregated across other people's repositories.

- Format: [PublicTxt Format Specification](spec/publictxt-spec.md) (v0.1.0-draft)
- Live example: [jaysen.co.za](https://jaysen.co.za)
- Previous README: [docs/archive/README-2026-10-29.md](docs/archive/README-2026-10-29.md)

## Roadmap

| Version | Scope | Status |
|---|---|---|
| **v1 Personal** | Store and publish your own knowledge: Obsidian + Facet Plugin, PublicTxt-Pages deploy, format spec 0.1 | **Current** |
| **v2 Aggregation** | Subscribe to other PublicTxt repos and browse a filtered, merged view | In progress |
| **v3 Community** | Groups, shared repos, discussion, federation | Designed |

## v1 Personal - Store and Publish

> "I keep my notes and writing in a Git repository, edit them in Obsidian, and publish them to the web for free with search and tag navigation."

### How it fits together

1. **Write** in Obsidian, using relative Markdown links.
2. **Navigate** your vault by tag combinations with the Facet Plugin.
3. **Commit and push** to GitHub, GitLab or any Git host.
4. **Publish** with PublicTxt-Pages to GitHub Pages or any static host.

No server, no database, no paid service. Your content stays plain Markdown that outlives the tooling.

### Obsidian PublicTxt Facet Plugin - v1.0

[github.com/publictxt/obsidian-publictxt-facets](https://github.com/publictxt/obsidian-publictxt-facets)

- Search and filter notes by combinations of tags, ratings, category and collection facets.
- Selecting a tag narrows the remaining tags to those that co-occur with the selection, so drill-downs never dead-end.

### PublicTxt-Pages

[github.com/publictxt/publictxt-pages](https://github.com/publictxt/publictxt-pages) - zero-backend static site for a PublicTxt repo or any Obsidian / Markdown vault.

- **Static and free**: Hugo build, deploys to GitHub Pages or any static host.
- **Works on plain Obsidian vaults**: relative Markdown links build as-is, no conversion step.
- **Search and facets**: full-text search and filtering by tags (incl. combinations) and document type via [Pagefind](https://pagefind.app/), with no hosted search.
- **Hashtags**: inline `#tags` become front matter tags at build time.
- **Content types**: wiki, blog, notes and bookmarks, from folder layout or front matter `type`.
- **Sidebar**: sections, top tags / tag cloud, page metadata.
- Single repository per site.

### Optional: publish from a private vault

Keep a private Obsidian vault and copy only selected notes into your public repo with [WikiTool](https://github.com/jaysen/WikiTool). It selects by tag expression (e.g. `public - draft`), search or attributes, and converts Obsidian wikilinks to Markdown links. Edit imported notes in the private vault and re-run the copy.

### Repository layout

Default folders (spec §4 is authoritative; a `publictxt.yaml` manifest can remap them):

| Path | Type |
|---|---|
| `wiki/` | Wiki pages |
| `blog/<yyyy>/<mm>/<yyyymmdd>[-<n>]-<slug>.md` | Blog posts |
| `notes/` | Notes |
| `metaweb/<domain>/<subdomain-or-folder>/<file>.md` | Bookmarks, one file per web resource |
| `media/` | Attachments |
| `publictxt.yaml` | Optional manifest |

- Any document with a `bookmark: <url>` front matter attribute is also a bookmark.
- Indexes and tag pages are derived data and are never committed.

## v2 Aggregation - Follow Other Repos

The differentiator: pull chosen subsets of other people's PublicTxt repos into a local, searchable view.

- Repos stay single-author; multi-author views come from aggregation.
- Subscribe via `follows` in `publictxt.yaml`, filter at query time by tag, path or type.
- Reconcile tag vocabulary at view time with your own alias file - no global normalization.
- Bookmarks of the same resource group across repos by `resource_id`.
- Foundation for [CuratedCommons](https://github.com/publictxt/curated-commons): social bookmarking and curation across PublicTxt repos.

Open design questions are tracked in [spec §13](spec/publictxt-spec.md#13-open-questions).

## v3 Community - Later

- Groups, shared repos, profiles, discussion.
- Public / semi-public / private content controls.
- Friendlier merge UX for non-technical contributors.
- ActivityPub, MetaWeb browser extension, W3C Web Annotation export.
- Richer data models: semantic edges, graph database backends, reputation.

## Related Tools

| Repo | Purpose | Status |
|---|---|---|
| [WikiTool](https://github.com/jaysen/WikiTool) | .NET wiki libraries; converts between wiki syntaxes (incl. Obsidian ↔ Markdown links); copy by search, tag expressions, attributes | In progress |
| [PublicTxt.net](https://github.com/publictxt/publictxt.net) | .NET implementation: core, syntax, Git and data layers; Avalonia and Blazor apps later | In progress |
| [CuratedCommons](https://github.com/publictxt/curated-commons) | Curation and bookmarking layer over PublicTxt repos ([details](wiki/publictxt/CuratedCommons.md)) | Specification |

## License

GPLv3 - see [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
