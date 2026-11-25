---
fork_remote: publictxt-remotes
fork_repo: https://github.com/publictxt/obsidian-publictxt-remotes
fork_path: docs/features/fork.md
fork_branch: master
fork_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
fork_blob: 5fe2913bc870f99e7de3f37706f8ada96f08e8b6
fork_date: 2026-11-25T23:51:24+02:00
---
# publictxt-remotes plugin - Fork feature

A fork is the user's own note made from an upstream page. They edit it; upstream changes come
through Compare ([fork-update.md](publictxt-remotes%20fork-update.md)).

## Provenance

Flat front matter keys, so Properties can show them. Upstream front matter is kept.

| Key | Value |
|---|---|
| `fork_remote` | The source's remote name |
| `fork_repo` | Canonical repo URL; hosted sources only |
| `fork_path` | Repo-relative path forked |
| `fork_branch` | Branch forked from |
| `fork_commit` | Head commit at fork time |
| `fork_blob` | Blob at fork time: change marker and merge base |
| `fork_date` | ISO datetime with offset |
| `fork_seen` | Upstream blob dealt with without taking it, or `gone`. Absent until then |

A note is a fork when `fork_path` and `fork_blob` are valid and it has a valid `fork_remote` or
`fork_repo`. Writing a fork sets these and removes other `fork_*` and all `mirror_*` keys, so a
fork of a fork never keeps upstream's provenance. A fork (or mirror) belongs to a source by
`fork_remote` (case-insensitive), else `fork_repo` (`isFrom()`).

## Ways in

**Fork** on a file in What's new, All files, or a mirror gone upstream (at the check's head);
**Fork selected**; **Fork a file from a source**; **Fork this mirror** (active note, no fetch).
Markdown only.

**A mirrored file is moved, not fetched**: `renameFile` (Obsidian updates links to it; mirrors and
the moved note get their text back, `moveMirror()`), keys `mirror_*` → `fork_*`, `fork_date` now,
text unchanged but for its links. Never copied: two notes of one name would split every link by
name. Its own path doesn't count as taken. Of a file mirrored twice (copied by hand), **Fork this
mirror** moves the active note, anything else the first by path. If upstream has moved on, the
next check lists it under Forks.

## The modal

One modal for one file or several.

- **Folders**:
  - **Into the folder**: each file into **Folder** under its own name.
  - **Into the folder, keeping source folders**: `wiki/cs/git.md` + `notes/research` →
    `notes/research/wiki/cs/git.md`.
  - **Same folder as in the source**: the same path in the vault.

  Missing folders are created. Fixed at **Into the folder** when no file has a folder.
- **Folder**: empty is the vault root. Suggests up to 50 vault folders; free text. End slashes
  dropped; `.`/`..` parts or a file path are errors.

Opens on **Same folder as in the source** when `matchFolders` is on and any file has a folder;
else **Into the folder**. **Folder** starts as `forkFolder`. The modal says so until edited. The
typed folder survives switching choices. The modal doesn't stop a fork going under the community
folder.

**One file** also has **Name** (no `.md`): the source name, or the first free `<name> 1`, `2`, …
with a note saying so. It follows the destination until typed in; then it must be free. Empty,
`/` or `\` is an error. Earlier forks of the file are named.

**Convert links** and **Match links to existing pages**, applied once the forks are written:
[fork-links.md](publictxt-remotes%20fork-links.md).

**Preview**, redrawn on change: "Goes to `<path>`", or a row per file (`source → vault`, "Name
taken." when numbered (case-insensitive), "Forked before.", "Its mirror moves."), plus folders
to be made. An error replaces it and disables **Fork** / **Fork all**. Enter forks. Nothing is
overwritten.

## Writing

Per unmirrored file: fetch blob, create note, add keys (`processFrontMatter`). Invalid upstream
YAML is refused before writing.

- One file: notice, the note opens. Failure shows in the modal, which stays open.
- Several: each in turn, continuing past failures; one notice (sticky if any failed); nothing
  opened. Ticks are cleared.

## Code

Pure: `src/fork/destination.ts`, `fork.ts`, `src/copy/copy.ts`, `src/sources/fork-meta.ts`,
`src/check/selection.ts`. Vault: `src/fork/run.ts`, `command.ts`, `src/copy/run.ts`,
`src/sources/forks.ts`. UI: `src/ui/fork-modal.ts`, `fork-preview.ts`, `folder-suggest.ts`.
