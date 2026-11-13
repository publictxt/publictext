# 0017. Mirrors: followed repos' pages, as written, under `community/`

- **Status:** Proposed
- **Date:** 2026-11-13

## Context

- Taking a page from a followed repo was one step: fork it (ADR-0012). The fork is the author's own note from that moment, with its links converted and upstream's changes arriving as a compare.
- Most pages an author wants at hand they want to read, search and link to, not edit. Forking those makes each a note to maintain, and blurs which pages are the author's.
- Bringing a whole followed repo into the vault would make it searchable, at the cost of size and of drowning the author's own notes.
- obsidian-publictxt-remotes had a `watch` list: files reported on when they change, without a copy. It answered "tell me when this moves" and nothing else.
- A page copied out of its repo keeps working links only while it keeps its place among its neighbours.
- The folder such copies live in is a section of the published site, so its name is read by visitors, not only by tools.

## Decision

- Three steps: **follow** a repo (ADR-0014), **mirror** the pages worth having at hand, **fork** the ones to make one's own.
- A mirror is the source's document, unchanged: its front matter, body and links as written there. It carries `mirror_*` keys, the namesakes of `fork_*` without `fork_seen` (spec §5.5).
- Mirrors live at `community/<follow name>/<path in the source>` (spec §4.1). One folder per follow keeps sources apart; keeping each source's layout keeps the links between its mirrors working.
- A mirror is updated by overwriting it with the source's text. No compare and no merge: the copy is the source's.
- Forking a mirror moves it into the author's notes and renames its keys to `fork_*`. Links are converted then, once, when the note leaves the source's layout.
- The keys say what a document is, not the folder, so a published document says so too and a moved one doesn't change kind. Valid `fork_*` keys win.
- The folder is `community/`: the word PublicTxt-Pages already uses for where pages came from. Not `mirrors/` (the mechanism, and on a site it reads as download mirrors) or `remotes/` (git's word for the repos, not their pages).
- Source and source rating (ADR-0016) read mirrors as they read forks.
- A third computed value, copy, tells the two apart: `forked`, `mirrored`, or none for the author's own documents (spec §5.4). It goes by the keys, as the rest does, so it holds for a mirror kept outside `community/` and a fork filed under it. Like the source, it is never written to disk (ADR-0003).
- `watch` is removed: a mirror is a watched file the repo also holds.

## Consequences

- + The author's notes and other people's stay visibly apart: everything under `community/` is someone else's text.
- + Mirrored pages are searchable, linkable and show backlinks, without the whole repo.
- + A published site can show a mirror as "from *source*" and a fork as "forked from *source*", and can leave `community` out of lists by default.
- + Views can filter on copy: only the author's own versions (`forked`), or without other people's pages (`mirrored` excluded), wherever the files are. PublicTxt-Pages does (`?copy=`).
- + Links are rewritten only for forks, so a mirror never drifts from its source.
- − `community/` was pencilled in for profiles, groups and discussion (README, spec §13). Those need a place: beside the mirrors, or another folder.
- − A mirror's links to pages that aren't mirrored are unresolved, and root-relative ones resolve against this repo (spec §13).
- − Only Markdown is mirrored for now: a mirror's images don't come with it.
- − Republishing someone's page whole is a bigger step than linking to it. It is public text in a public repo under its licence, and the keys say whose it is, but views should label it plainly.
- ! obsidian-publictxt-remotes' clones folder is renamed `.clones/` (was `.remotes/`, ADR-0011, ADR-0014), so it isn't taken for a folder of pages.
