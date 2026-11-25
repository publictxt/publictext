---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/user/writing.md
mirror_branch: main
mirror_commit: 184a8535c03384a65b94a3ef851e0127c1b99085
mirror_blob: f374799fc0613c346c4565b185b02e1daffc521a
mirror_date: 2026-11-25T22:44:54+02:00
---
# Writing

How your notes become pages. Write normal Markdown; the build fills in what you leave
out (titles, dates, tags). Front matter (Obsidian calls it **Properties**) is always
optional.

- [Folders and sections](#folders-and-sections)
- [Titles](#titles)
- [Dates](#dates)
- [Tags](#tags)
- [Collections](#collections)
- [Categories and ratings](#categories-and-ratings)
- [Links](#links)
- [Images, audio and video](#images-audio-and-video)
- [Embedding notes](#embedding-notes)
- [Tag lists (Dataview)](#tag-lists-dataview)
- [Bookmarks](#bookmarks)
- [Posted elsewhere](#posted-elsewhere)
- [Follows, mirrors and forks](#follows-and-forks)
- [Drafts and hidden pages](#drafts-and-hidden-pages)
- [Front matter reference](#front-matter-reference)

## Folders and sections

- Every `.md` file is a page; every folder is a section with its own page listing
  what's in it.
- **Top-level folders** (`blog/`, `wiki/`, …) appear in the sidebar. Root-level notes
  are plain pages, outside any section.
- **`_index.md`** is a folder's own page: its intro text, shown above the folder's list
  and searchable. At the root, it's the home page. No `_index.md`? One is generated.
- **A folder note** can be the folder's page instead: any note with
  `is-folder-index: true` in its front matter, such as Obsidian's
  `Design notes/Design notes.md`. Its title is its own name, the edit link opens it,
  and links to it reach the folder's page. A real `_index.md` wins, as does the first
  flagged note by name; the build warns about the other, which stays a plain page.
- An `_index.md` can carry `order:` / `pagerSize:` / `filter:` for its list
  ([Configuration](configuration.md#browse-lists)).
- Folder names show as written (`Design notes`, not `design-notes`).

```txt
_index.md                       home
Roadmap.md                      a root page
blog/_index.md                  "Blog" intro
blog/2026/20260509-hugo.md      a post; date from the name
wiki/Site/_index.md             a sub-section's intro, any depth
wiki/Site/Search.md
```

Sub-folders are for *you* — organise however you like. Only the top level shapes the
navigation.

## Titles

First match wins:

1. `title:` in front matter
2. A `# Heading` on the first line (blank lines before it are fine) — removed from the
   body so it isn't shown twice. A `# Heading` further down stays a heading.
   Turn this step off with [`titleFromHeading`](configuration.md#titles-from-headings).
3. The file name

Obsidian's "inline title" is the file name, so a note with neither is titled as it
appears in Obsidian.

## Dates

Every page gets **created** and **updated**. Created, first match wins:

1. `created:` (or `date:`) in front matter — `2026-10-02` or `2026-10-02T09:30:00+01:00`
2. A date in the file name — `20261002-title.md`, `2026-10-02-title.md`,
   `Title . 20261002.md` — or the path `blog/2026/10/02/title.md`
3. The commit that first added the file
4. The file's timestamp

Updated: `updated:` (or `lastmod:`), else the last commit touching the file.

Practical upshot: **you rarely need to type dates.** Git history supplies them. Name
posts with a date prefix when the date is part of what they are (a log, a journal);
leave reference notes undated and let Git track them.

Obsidian tip: Daily Notes and the Templates plugin can write `created: {{date}}` for you.

## Tags

Two ways, merged into one list:

- **Inline**: `#hashtag` anywhere in the text. Becomes a link to the tag's page.
- **Front matter**: `tags: [hugo, search]` or Obsidian's Tags property.

Rules:

- Starts with a letter; then letters, digits, `_`, `-`. Shown lower-case.
- Not tags: anything in `code`, URLs (`page#section`), links, `C#`, `issue #42`.
- Obsidian's nested tags (`#books/fiction`) work, and also tag the page with their
  parents: it shows `#books` and `#books/fiction`, and `#books` lists, counts and
  filters it. A nested tag's page is `/tags/books--fiction/`, so don't also use a
  literal `books--fiction` tag.

Each tag gets a page (`/tags/hugo/`), a place in the sidebar cloud, and a filter chip.

## Collections

A collection groups pages across folders. Every page is in:

- its **top-level folder**'s collection (`blog/…` → `blog`; root pages → `page`), plus
- anything in `collections:` — a value or a list, plus
- `bookmarks`, if it has a `bookmark:` URL.

```yaml
---
collections: [blog]    # filed in posts/, listed under Blog too
---
```

A top-level section lists every page in its collection, wherever filed: `/blog/` shows
`posts/hello.md` above. Collections without a folder (`collections: [recipe]`) still
get chips and filters, just no section page. Case doesn't matter.

**Collections vs tags vs categories** — rule of thumb:

| | Vocabulary | Use for |
|---|---|---|
| Collection | Your folders, plus a few extras | What *kind* of thing: post, wiki page, bookmark |
| Category | Closed list in config | A handful of broad subjects |
| Tag | Anything | Topics, as many as you like |

## Categories and ratings

```yaml
---
category: Tech                  # or categories: [Tech, Personal]
rating: 4                       # 1–5
---
```

Categories must be on the [configured list](configuration.md#categories) (else ignored,
with a build warning). Ratings show as stars and drive *Top rated* sorting and a
minimum-rating filter.

## Links

Use relative Markdown links — Obsidian writes these with the setup in
[Setup step 3](setup.md#3-configure-obsidian):

```markdown
[Search](../wiki/Site/Search.md)
[A post](<2026/20260925 A note with spaces.md>)
[Same, escaped](2026/20260925%20A%20note%20with%20spaces.md)
[A heading](Search.md#facets)
```

- `.md` links become page links; they also work on GitHub and in any editor.
- File names with spaces need `<…>` or `%20` — Obsidian writes `%20`.
- `[[wikilinks]]` are **not converted**; they show as literal text.
- Links to hidden or excluded pages, or to notes that don't exist, are left as dead
  links. Check before hiding a page others link to.

## Images, audio and video

Obsidian's image syntax does all three:

```markdown
![A diagram](diagram.png)                          image
![A recording](../media/talk.mp3)                  audio player (mp3, m4a, ogg, opus, wav, flac, aac)
![A talk](https://www.youtube.com/watch?v=abc123)  YouTube player (youtu.be links and ?t= work too)
```

Where files must live, relative to the note:

- **Beside it** — the same folder, or a subfolder (`attachments/`), or
- **In a top-level folder** of files, like `media/`.

Anywhere else, the build leaves the link as written and warns. Obsidian's
*Default location for new attachments: Same folder as current file* handles this —
pasted images land beside the note.

No need for one-folder-per-post: `blog/20260925-post.md` and
`blog/Pasted image 20260925.png` side by side is fine.

## Embedding notes

A note in image syntax, **on its own line**, embeds its content in a box — as
Obsidian's `![[note]]` does:

```markdown
![](../wiki/Site/Search.md)
```

- Inline in a sentence, it's a plain link.
- One level deep: embeds inside an embedded note become links.
- Heading embeds (`page.md#section`) are a link for now.

## Tag lists (Dataview)

A small subset of [Dataview](https://blacksmithgu.github.io/obsidian-dataview/) renders
on the site, so one block works in both places:

````markdown
```dataview
LIST FROM #recipes
LIMIT 10
```
````

Lists pages with that tag, recently updated first. `LIMIT` is optional. Any other
query (`TABLE`, `WHERE`, `SORT`, several tags) shows as a code block on the site — it
still works in Obsidian.

## Bookmarks

A page with a `bookmark:` URL is a bookmark, wherever it lives — it joins the
`bookmarks` collection and its link shows as a chip.

```yaml
---
bookmark: https://gohugo.io
tags: [hugo, ssg]
---
Why it's worth reading…
```

`bookmarks:` takes a list. The convention: one file per resource under
`bookmarks/sites/<domain>/`, and pages *about* bookmarks under `bookmarks/wiki/`.
A page elsewhere under `bookmarks/` without a URL gets a build warning.

## Posted elsewhere

Cross-posted? Add the URLs; they show as "Posted on" links:

```yaml
---
mastodon: https://mastodon.social/@you/1130000
substack:
  - https://you.substack.com/p/part-1
  - https://you.substack.com/p/part-2
---
```

Default keys: `facebook`, `twitter`, `substack`, `mastodon`, `github`. Add others in
[Configuration](configuration.md#posted-on-links).

## Follows and forks

Tools such as obsidian-publictxt-remotes follow other repos, mirror pages from them and
fork pages from them; the site reads what they write.

- **A follow** is a note in `follows/` with the repo's `url` (or `local: true`, for a
  repo with no public URL). The file name is the follow's name; its `rating` is your
  rating of that repo — the [source rating](site.md#community) of every page mirrored or forked from it —
  and its `description` a line about it. The note's page shows the repository, the
  rating and links to its mirrored and forked pages; the note itself counts as unrated.
- **A mirror** is a page from a repo you follow, kept as its author wrote it, with
  `mirror_*` keys saying where it came from. Mirrors live in the community folder
  (`community/`, or [`communityFolder`](configuration.md#browse-lists)), in the follow's
  mirror folder: its `folder:`, else its name. It shows **From** — the follow's name, linked to its note — and
  **↗ original**, as a fork does. Its `rating`, if it has one, is its author's. Mirrors are
  [left out of lists and search](site.md#community) until a reader asks for them.
- **A fork** is your own page, made from one in a repo you follow, with `fork_*` keys saying where it was copied from. It shows
  **Forked from** — the follow's name, linked to its note — and **↗ original**: the file
  it was forked from on GitHub, GitLab or Codeberg; elsewhere the repo; for a local
  follow, nothing.

```yaml
---
# follows/ann-notes.md
url: https://github.com/ann/notes
rating: 4
description: Ann's wiki on federation.
---
Why I follow Ann, and what to read there.
```

A fork keeps its date if it has one of its own — `created:`, or a date in the file name.
Without one, its date is when it came into your repo, shown as **forked** *date*: the
original's history isn't in your repo. A mirror is the same, shown as **mirrored** *date*.

The mirroring tool points a mirror's links at where each linked page's mirror is or would
be, by full path (`[b](community/ann-notes/cs/b.md)`). A link to a page that wasn't
mirrored goes to the original instead: at the commit the linking page came from, else
the follow's `branch:`, else the repo. A local follow's stay as written.

A fork whose `fork_remote` names no follow — the follow was renamed or removed — gets a
build warning (a mirror's `mirror_remote` likewise), and shows that name unlinked.

## Drafts and hidden pages

```yaml
---
publish: off      # also: false, no, 0
---
```

- The page isn't built.
- A folder whose notes are *all* unpublished keeps its other files (images) off too —
  so a draft post folder stays fully hidden. Beside a published note, files publish.
- Whole folders: [`excludeFolders`](configuration.md#leave-folders-out).

**Hidden is not private.** The note is still in your public repo, readable on GitHub.
Keep anything truly private out of the repo — a separate vault, or a `.gitignore`d
folder.

## Front matter reference

All optional.

| Key | Value | Effect |
|---|---|---|
| `title` | text | Page title (else an H1 on the first line of the body, else file name) |
| `created` / `updated` | date or date-time | Override Git-derived dates (`date` / `lastmod` also read) |
| `tags` | list | Merged with inline `#hashtags` |
| `collections` | value or list | Extra collections — see [Collections](#collections) |
| `category` / `categories` | value or list | From the configured list |
| `rating` | 1–5 | Stars; rating sort and filter |
| `bookmark` / `bookmarks` | URL or list | Makes it a bookmark |
| `facebook`, `mastodon`, … | URL or list | "Posted on" links |
| `fork_remote`, `fork_repo`, `fork_path`, … | written by the forking tool | A fork — see [Follows and forks](#follows-and-forks) |
| `mirror_remote`, `mirror_repo`, `mirror_path`, … | written by the mirroring tool | A mirror — see [Follows and forks](#follows-and-forks) |
| `url`, `local`, `rating` in `follows/` | repo URL, `true`, 1–5 | A follow, and your rating of it |
| `publish` | `off` | Keep off the site |
| `is-folder-index` | `true` | This note is its folder's page, in place of `_index.md` — see [Folders and sections](#folders-and-sections) |
| `description` | text | The page's meta description (search engines, link previews) |
| `order` / `pagerSize` | see [config](configuration.md#browse-lists) | On `_index.md`: this folder's list order and page size |
| `author`, `source_repo` | text | Kept, not shown yet |

Any other key is kept and ignored — your own Dataview fields are safe. The site
computes `source_name`, `source_rating`, `source_follow`, `source_original`,
`follow_name` and `follow_url` itself: written in a note, they are dropped.
