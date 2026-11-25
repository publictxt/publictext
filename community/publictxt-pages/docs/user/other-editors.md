---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/user/other-editors.md
mirror_branch: main
mirror_commit: 184a8535c03384a65b94a3ef851e0127c1b99085
mirror_blob: e4e5250c75335e9fdaaa74819220f8446cd72381
mirror_date: 2026-11-25T22:44:54+02:00
---
# Other editors

The site reads a folder of plain Markdown in Git. Nothing ties it to Obsidian; any
editor that writes `.md` files works, and you can mix them — Obsidian on the laptop,
the GitHub web editor from a phone.

## What any editor needs to get right

The same rules Obsidian is configured for in [Setup](setup.md#3-configure-obsidian):

- **Markdown links with relative paths** — `[text](../wiki/page.md)`, not
  `[[wikilinks]]`.
- **Attachments beside the note**, or in a top-level folder like `media/`.
- **YAML front matter** between `---` lines, if any.
- **A blank line between paragraphs** — single line breaks join up.

Everything else in [Writing](writing.md) is plain text conventions any editor can
type.

## Editors

| Editor | Fit | Notes |
|---|---|---|
| **Obsidian** | Default | Graph, backlinks, Properties UI, Dataview preview; Git via the Obsidian Git plugin |
| **VS Code** (+ [Foam](https://foambubble.github.io/foam/) optional) | Good | Built-in Git and Markdown preview; drag-in images land beside the note. Foam adds backlinks — set its link format to Markdown links, not wikilinks |
| **GitHub web editor** | Quick edits | The site's *Improve this page* link opens it. Press `.` on the repo for the full web VS Code. Commits publish directly |
| **Typora, Zettlr, MarkText** | Good | WYSIWYG Markdown; set image paths to relative, copy images beside the note. Commit with GitHub Desktop |
| **Logseq** | Poor fit | Outliner format and `[[links]]` by default; pages read as bullet lists |
| **Any text editor** | Works | vim, Notepad, iA Writer — plain files, then commit |

## Mixing editors

- Commit `.obsidian/` only if you want shared Obsidian settings; other editors ignore
  it, and so does the site.
- Watch for editors that rewrite links (to wikilinks, or absolute paths) on rename.
  Run a [local build](setup.md#optional-preview-locally) after bulk renames: the build
  warns about images it can't place, and a dead link shows as a link to nowhere.
- A Dataview block shows as code outside Obsidian — harmless.

## Coming from somewhere else

| From | Notes |
|---|---|
| Another Obsidian vault | Turn wikilinks off, then convert existing ones (community plugins can bulk-convert wikilinks to Markdown links) |
| Jekyll / Hugo blog | `date:` and `lastmod:` are read as `created` / `updated`; `title:` and `tags:` carry over. Move posts into `blog/` |
| Notion / Bear / Evernote export | Exports are Markdown with attachments in subfolders — usually works; check links to other pages |
