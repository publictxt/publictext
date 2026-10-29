---
tags: [publictxt, docs, publictxt-pages]
---
# Publishing a PublicTxt site

A PublicTxt repository becomes a website with [PublicTxt-Pages](PublicTxt-Pages.md) and GitHub Pages: no server, no database, no fork of the site generator. This site and [jaysen.co.za](https://jaysen.co.za) are built this way.

The full guide is [PublicTxt-Pages setup](https://github.com/publictxt/publictxt-pages/blob/main/docs/user/setup.md). In short:

## How it works

On every push, a GitHub Actions workflow in the content repository:

1. checks out the content repo with full history (page dates come from Git),
2. checks out [publictxt-pages](https://github.com/publictxt/publictxt-pages),
3. installs Hugo and Node (for Pagefind),
4. runs `python3 site/scripts/build.py --source ../txt` — sync, Hugo build, search index,
5. deploys `public/` to GitHub Pages.

The content repo stays pure Markdown. Upgrading the site is a change of `ref:` in the workflow, or a re-run.

## Setting up a repository

1. Lay it out as in [Repository structure](Repository-Structure.md).
2. Copy [`deploy/publish-to-github-pages.yml`](https://github.com/publictxt/publictxt-pages/blob/main/deploy/publish-to-github-pages.yml) to `.github/workflows/publish.yml`, and set the branch it publishes from.
3. In the workflow's Build step, set `HUGO_BASEURL` to the site's address, with a trailing slash.
4. Put site settings (title, description, filter chips) in `settings/site.toml`; it's merged over PublicTxt-Pages' `hugo.toml`. `HUGO_*` env vars in the workflow win over both.
5. On GitHub: **Settings → Pages → Source: GitHub Actions**. Push.

Every push to the publishing branch republishes. **Actions → Publish site → Run workflow** rebuilds without a push.

## This repository

- Workflow: [`.github/workflows/publish.yml`](https://github.com/publictxt/publictext/blob/main/.github/workflows/publish.yml), publishing from `main`
- Settings: [`settings/site.toml`](https://github.com/publictxt/publictext/blob/main/settings/site.toml)

## Preview locally

Needs Python 3.11+, Hugo 0.162+, and Pagefind or Node.

```bash
git clone https://github.com/publictxt/publictxt-pages
cd publictxt-pages
python3 scripts/build.py --source ../publictext --serve
```

Pushing is publishing: everything in a public repository is public, including pages marked `publish: off`.
