---
bookmark: https://github.com/publictxt/publictxt-pages
tags: [publictxt, publictxt-pages, docs]
---
# PublicTxt-Pages

A zero-backend static site for browsing and searching a PublicTxt repository, or any Obsidian / Markdown vault. Formerly Txt-Hugo. This site is built with it — see [Publishing](Publishing.md).

## What it does

- **Static and free to host** — Hugo build, deployed to GitHub Pages or any static host. No server, no hosted search.
- **Works on plain Obsidian vaults** — relative Markdown links build as a working site with no conversion step.
- **Full-text search and facets** — [Pagefind](https://pagefind.app/) search; filter lists and results by collection and tags, including combinations, with include / exclude chips.
- **Hashtags** — inline `#hashtags` merge with front matter `tags`; nested tags (`dev/web`) too.
- **Collections** — top-level folders (wiki, blog, posts, notes, bookmarks) plus `collections:` and `bookmark:` front matter. See [Repository structure](Repository-Structure.md).
- **Dates from the path and Git** — `created` / `updated` derived, no front matter needed.
- **Navigation** — sections, tag cloud, folder trees for the wiki, timelines for the blog.

## How

- Python preprocessing (no dependencies): content sync, titles, dates, hashtags
- Hugo templates; browse lists and facets in plain ES modules, bundled by Hugo
- Pagefind for search

## Links

- [Repository](https://github.com/publictxt/publictxt-pages)
- [User guide](https://github.com/publictxt/publictxt-pages/tree/main/docs/user)
- [Spec](https://github.com/publictxt/publictxt-pages/blob/main/docs/SPEC.md)
- Live: this site, [jaysen.co.za](https://jaysen.co.za)
