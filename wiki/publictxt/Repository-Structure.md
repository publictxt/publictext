---
tags: [publictxt, docs, spec]
---
# Repository structure

How a PublicTxt repository is laid out, following the [PublicTxt-Pages content structure](https://github.com/publictxt/publictxt-pages/blob/main/docs/SPEC.md#content-structure). The full PublicTxt format specification is still to be written; until then, this is the working layout.

```txt
_index.md                  home page
wiki/                      reference pages, read by place
  _index.md                a folder's own page (any depth)
blog/                      dated posts; the date comes from the path
  2023/12/17/post.md         day folder
  2024/09/20240922-title.md  YYYYMMDD prefix
  20260509-title.md          YYYYMMDD prefix, no folders
posts/                     longer pieces
notes/                     short notes
bookmarks/
  sites/<domain>/page.md   one file per web resource, with a `bookmark:` URL
  wiki/topic.md            pages about bookmarks (no URL needed)
media/                     shared images and audio, copied as-is
settings/site.toml         site settings for PublicTxt-Pages (optional)
.github/workflows/         the publish workflow
```

Never published: `README.md`, `LICENSE`, `CONTRIBUTING.md`, `CNAME`, `.obsidian/`.

## Collections

Each top-level folder is a **collection** (and a section of the site). A page can join others:

- `collections: [blog]` in front matter — listed under Blog too, wherever it's filed
- a `bookmark:` URL — joins `bookmarks`, wherever it lives

Collections add, never replace: a page always stays in its folder's collection.

## Front matter

All optional; the build derives the rest.

| Key | Purpose |
|---|---|
| `title` | Falls back to the first `# H1`, then the filename |
| `tags` | Merged with inline `#hashtags` |
| `collections` | Extra collections |
| `bookmark` / `bookmarks` | The URL(s) the page is about |
| `created` / `updated` | Override dates from the path and Git history |
| `publish: off` | Leave the page off the site (it's still public in the repo) |

## Links and media

- Link with relative Markdown links: `[text](../wiki/page.md)`. `[[wikilinks]]` show as plain text.
- Put images beside the note, or in a top-level folder like `media/`.

Changing a rule here? It follows the PublicTxt-Pages spec; change that first.
