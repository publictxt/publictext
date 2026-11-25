---
tags:
  - web
  - dev/web
  - publictxt
  - cs
project:
  - "[[PublicTxt]]"
mirror_remote: pwiki
mirror_path: 01-projects/publictxt/projects/PublicTxt-Pages/PublicTxt-Pages.md
mirror_branch: master
mirror_commit: 06d1c3f941b51c1996858c09b63767ac1dd459d5
mirror_blob: e11bc9ebcc0fde3a5bb9bc48c0e117b5ada43d99
mirror_date: 2026-11-25T23:11:51+02:00
---
Statically-hosted web interface for [PublicTxt](https://github.com/publictxt/publictxt-pages/blob/publictxt) repos — deployable to GitHub/GitLab Pages for free, with no backend required.
Python preprocessing + Hugo build produce the static site; a dynamic client-side layer (Pagefind + JS) adds full-text search and faceted tag/type browsing on top.
Works directly against a standard Obsidian-style Markdown vault — relative links, inline `#hashtags`, and folder-derived types — with minimal reliance on PublicTxt-specific syntax transforms.

## Tasks
### Features:
- [ ] [[PublicTxt-Pages Process WikiLinks]] #task-publictxt 
- [x] [[PublicTxt-Pages Index pages by attribute]] #task-publictxt 
- [ ] Sort by Author
### Fixes


## Archived Tasks
### Fixes
- [x] blog dates not working for some where perhaps date in title only and other cases
- [x] Created/Updated dates are wrong
- [x] Change back to .Date and .Lastmod (Hugo defaults)

### Features
- [x] Title only to use H1 on first line of body
- [-] Convert pages with images and attachments into [[Hugo Content Bundles]] - will fix Images
- [x] Add Categories
- [x] Sort by Year/Month/Type/Category


## Early Notes
- [[Obsidian to Hugo]]
- [[Hugo]] Templates for [[PublicTxt]] content
- [[MetaWeb]] templates
	- mimic dynamic pages for browsing and filtering 
	- tag clouds 
	- use [[Obsidian Dataview Properties]]

-----

# Spec

## Principles
- **No PublicTxt.Syntax / .NET dependency.** Works with real wikis: relative links, no front matter required.
- Source repo never modified; build operates on generated copy `build/content/`.
- Links are `[label](../wiki/page.md)` - No `[[wikilinks]]`
- Hugo renders `.md` extensions to `.html`

## Feature list
- Sections
	- Single repo;  top-level folders are sections (`wiki/`, `blog/`, `notes/`, `bookmarks/`, `posts/`, …). Root-level `.md` → plain pages.
	- home list sections by folder name, ordered by `params.sectionOrder` *(D10)*.
	- Section index bodies render as prose and are search-indexed.
- Sidebar
	- Tag cloud 
	- page meta (type, dates, tags) in sidebar.
	- Author 
- Categories (TBD)
- Tags
	- Inline `#hashtags` merged into `tags`. Facets: tags (AND-able), type.
	- tag pages
- Browse Lists
	- All browse lists (home *Recent*, sections, tag pages) rendered client-side from one site-wide JSON index; plain `<ul>` no-JS fallback *(D6, D7)*.
	- Sorting in all by Date, Recency, Alphabetical.
	- Sort by Year/Month/Type/Category (TBD)
	- Sort by Source/Author (TBD)
	- Sort by Rating (TBD)
- Search
	- Full text search
	- Type and Tag filters as 'facets' are available without a query
	- Date filters (TBD)
	- Also Sortable
- Breadcrumbs from `.Ancestors` on all pages but home; folder names, date folders literal.
- Bookmarks
	- frontmatter 'bookmark' properties merged with bookmarks in section, without moving the page.
- Pages
	- Images and attachments to post should be converted to Hugo content bundles (TBD)

## Content Structure

```txt

index.md, Projects.md                   (home + root pages)

wiki/index.md, wiki/A/index.md          (folder index = section page (any depth))

blog/2023/12/17/x.md                    (dates derived from path)
blog/2024/home.md                       (home.md also acts as folder index)
blog/20260509-title.md                  (YYYYMMDD prefix date)
blog/2024/09/20240922-title.md          (YYYYMMDD prefix date)
blog/2026/09/22/Post-Name/title.md & blog/2026/09/22/Post-Name/image.png     (converts to Hugo content bundle)            

bookmarks/sites/domain/page.md     (one file per resource)

posts/post-name.md
notes/note-name.md

media/                             (non-Markdown copied verbatim)

README*, LICENSE*, CNAME, .git/    (skipped during sync)

```

## Front Matter Reference

All front matter optional; sync derives the rest. 

| Key                     | Purpose                    | Notes                                                                           |
| ----------------------- | -------------------------- | ------------------------------------------------------------------------------- |
| `title`                 | Page title                 | Falls back to first H1 (removed from body), then filename                       |
| `type`                  | Override folder-based type | Defaults to top-level folder name                                               |
| `tags`                  | Inline or block list       | Merged with `#hashtags`; deduplicated                                           |
| `created`               | ISO 8601 datetime          | Derived from path, git, mtime, or build time if not set                         |
| `updated`               | ISO 8601 datetime          | Derived from git, mtime, or build time if not set; never earlier than `created` |
| `author`, `source_repo` | Metadata                   | Passed through; not filterable                                                  |
| `bookmark`, `bookmarks` | URL or list                | Merges page into bookmarks section                                              |
Other keys pass through unchanged. `hugo.toml` maps `created` → `.Date`, `updated` → `.Lastmod`.

## Deployment
Build outputs to `public/`. GitHub Actions workflow (`deploy/publish-to-github-pages.yml`) checks out the content repo, runs the build pipeline, and deploys via `actions/deploy-pages`.




