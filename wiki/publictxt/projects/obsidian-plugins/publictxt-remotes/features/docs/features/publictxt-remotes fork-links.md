---
fork_remote: publictxt-remotes
fork_repo: https://github.com/publictxt/obsidian-publictxt-remotes
fork_path: docs/features/fork-links.md
fork_branch: master
fork_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
fork_blob: 37a131c8dee29aa17cae9e3de7c0f807bac19c6a
fork_date: 2026-11-25T23:51:24+02:00
---
# publictxt-remotes plugin - Fork links

A fork's links were written for upstream. Two things can be done about them, apart:

- **Converting** writes them as the vault writes links: wikilinks or Markdown links (**Use
  [wikilinks](../../../../../../../../forks/wikilinks.md)**). Their paths stay, so where they go is Obsidian's business: the wiki way.
- **Matching** points the broken ones at notes of the vault.

Neither looks at the source. Forks only, though matching works on any note but a mirror: a mirror's
links stay as written ([mirror.md](publictxt-remotes%20plugin%20-%20mirrors.md)).

**Which links**: the `links` and `embeds` Obsidian's metadata cache finds in the body, so code,
front matter and plain text are never touched. Of those, links to notes (`notePath()`): not web
links, links within the note, other files, or Markdown links without `.md` (perhaps a folder).

## Converting

`reformat()`: `[[../cs/b|see]]` ↔ `[see](../cs/b.md)`. The path, the shown words,
`#heading`/`#^block` and `!` are kept; a wikilink gets an alias only when it would show something
else, its `|` escaped in a table. Not followed: **New link format** (shortest, relative, absolute).

Left: one in that form already; one the other form can't hold (a title, words that can't be an
alias or a link's text); a Markdown link with a bare `)` in its path; a path with `| # ^ [ ]`; an
alias with `]`.

Paths hold up when forks keep the source's folders. Into one folder, links by name work and most
others don't.

## Matching

A link is broken when Obsidian finds nothing by it (`getFirstLinkpathDest`). One note in the vault
of its file name, without folder, `.md` or case (`nameOf()`): the link is pointed at it
(`relink()`), by the path the vault's settings give (`fileToLinktext`), in its form. None or
several: left. A link that works is never touched, even if it goes to another page than upstream
meant. A match by name may not be the page upstream meant either.

## When

- **After a fork** (a mirror that moves too): the notes just written, by the modal's **Convert
  links** (on each time) and **Match links to existing pages** (off each time); with both off,
  links stay as upstream wrote them. After **Overwrite** and **Write alongside**: converted, not
  matched. It waits up to 10 s for the metadata cache to see each; a sticky notice names any it
  couldn't do and why.
- **Convert links in this note** (`convert-links`): the active fork.
- **Match links in this note** (`match-links`): the active note, unless a mirror. A notice says how
  many were matched, have several matches, or have none.

A note is written once (`vault.process`), only if a link changed, and not at all if a cached link
position no longer matches the text. `fork_*` keys are untouched, so `fork_blob` stays the base,
and links changed so don't count as edits ([fork-update.md](publictxt-remotes%20fork-update.md)).

## Code

Pure: `src/fork/links.ts`, `src/copy/link-text.ts`. Vault: `src/fork/links-run.ts`.
