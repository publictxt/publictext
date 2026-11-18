---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/user/README.md
mirror_branch: main
mirror_commit: 8dde109855f4ca8323ea404f8327a48c1f497d3c
mirror_blob: 75507f2ed15377aadcc1b3861251d80604ad8dc9
mirror_date: 2026-11-17T21:07:18+02:00
---
# User guide

Publish a folder of Markdown notes as a browsable, searchable website — free, no server.

You write notes in a Git repository (your **content repo**). On every push, a GitHub
Action fetches **publictxt-pages**, builds the site, and publishes it to GitHub Pages.
Your repo stays plain Markdown: no theme, no build files, nothing to maintain.

```txt
 you, in Obsidian          GitHub                         readers
 ────────────────          ──────────────────────         ───────────
 my-notes/  ── git push ─▶ Action: fetch publictxt-pages ─▶ https://you.github.io/my-notes/
   blog/                   sync → Hugo → Pagefind
   wiki/                   deploy to Pages
```

## Guides

1. [Setup](community/publictxt-pages/docs/user/setup.md) — content repo, Obsidian, first publish, local preview
2. [Configuration](community/publictxt-pages/docs/user/configuration.md) — title, sections, filters, categories, and where each setting lives
3. [Writing](community/publictxt-pages/docs/user/writing.md) — folders, front matter, tags, links, embeds, dates, drafts
4. [The published site](community/publictxt-pages/docs/user/site.md) — what readers get: lists, filters, timeline, search
5. [Other editors](community/publictxt-pages/docs/user/other-editors.md) — VS Code, the GitHub web editor, anything that writes Markdown

## Obsidian is the default, not a requirement

Everything here is plain Markdown in folders. Obsidian is the recommended editor because
its vault *is* a folder of Markdown files and its link and property formats match what
the site reads — but any editor works. See [Other editors](community/publictxt-pages/docs/user/other-editors.md).

## Quick reference

| I want to… | Do this |
|---|---|
| Add a page | Create a `.md` file anywhere in the repo, push |
| Add a section | Create a top-level folder (`recipes/`) |
| Hide a page | `publish: off` in its front matter |
| Hide a folder | `params.excludeFolders` in `settings/site.toml` |
| Hide a file name (e.g. `README.md`) | `params.skipFiles` in `settings/site.toml` |
| Tag a page | `#hashtag` in the text, or `tags:` in front matter |
| Set the site title | `HUGO_TITLE` in the workflow, or `title` in `settings/site.toml` |
| Date a post | Name it `20260509-title.md`, or set `created:` |

Spec and internals (for contributors): [`docs/SPEC.md`](community/publictxt-pages/docs/SPEC.md), [`docs/wiki/`](community/publictxt-pages/docs/wiki/index.md).
