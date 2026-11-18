---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/sources.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 8b1327e8b79fcd2b4c8ba09b2eaf1856253d924d
mirror_date: 2026-11-17T21:07:37+02:00
---
# Sources and data

## data.json

| Field | Default | Meaning |
|---|---|---|
| `communityFolder` | `community` | Holds each source's mirror folder (`''`: vault root). publictxt-pages names the same folder in its own settings, set by hand |
| `forkFolder` | `''` | Where forks go when not to their source folder (`''`: vault root). Not under the community folder by default |
| `matchFolders` | `true` | Fork modal opens on **Same folder as in the source** when the vault has that exact folder |
| `listMirrors` / `listForks` | `false` | Keep a mirror / fork list in each follow note ([lists.md](community/publictxt-remotes/docs/features/lists.md)) |
| `clonesFolder` | `.clones` | Holds the clones; never empty. Hidden, so not indexed and skipped by Sync |
| `gitPath` | `git` | The git executable, for when Obsidian lacks the shell's PATH (macOS from Finder) |
| `localPaths` | `{}` | Remote name → absolute folder: a local source's, or a hosted source's local copy. Never in a note |
| `checkFrom` | `{}` | Remote name → `local` / `remote` for a hosted source with a local copy. Absent: `local`. Kept only for names in `localPaths` |
| `state` | `{}` | Source key → cache (below) |

`normalizeData()` turns anything loaded into valid data.

### State

A cache per source key; safe to delete (at worst everything shows as new). Dropped for
sources no longer followed at the next check.

| Field | Meaning |
|---|---|
| `seenCommit` | Head at the last acknowledged check. `null`: never checked |
| `checkedAt` | ISO datetime of the last check |

## Follow notes

A source is a note anywhere under the top-level `follows/`, one per repo (publictext ADR-0014,
spec §3.2). Sources are found by scanning, never stored. The body is the user's; the plugin
touches only front matter and its own list sections.

The file name without `.md` is the **remote name** (`follows/research.md` → `research`), which
mirrors and forks record. It can't start with `.` or a space or contain
`\ / : * ? " < > | # ^ [ ]`; unique, compared without case.

| Key | Type | Rule |
|---|---|---|
| `url` | string | Canonical repo URL. Required unless `local` |
| `local` | `true` | Plugin only. A local source: no `url`; folder in `localPaths` |
| `branch` | string | Branch to track. Absent/blank: the remote's default |
| `include` | string or list | Repo-relative folder prefixes ending in `/`. Empty: whole repo. Narrows "what's new" only |
| `folder` | string | Plugin only. Mirror folder under `communityFolder`. Absent: the remote name |
| `list_mirrors` / `list_forks` | boolean | Plugin only. Overrides `listMirrors` / `listForks` for this note |

`url`, `branch`, `include` are the publictext spec's; the rest are this plugin's, ignored by
other readers, and public with the note.

**Source key** (`Source.url`, used by state, clones and checks): the canonical URL, or
`local:<name>`.

**Reading** (`readFollow`): `local: true` makes a local source whatever its `url`; otherwise no
valid `url`, not a source. Paths: trailing `/` added to `include`, leading `/` stripped, `.`/`..`
segments dropped, lists deduped, a single string read as a list. Two notes with one URL or name:
the first by path (byte order) wins, the others show as duplicates.

### Canonical repo URL (v0)

`https://<host>/<owner>/<repo>`: scheme and host lower-cased; `.git`, trailing `/`, query and
fragment dropped; `git@host:owner/repo.git` → HTTPS form. Owner and repo keep their case. Anything
else is invalid, `file:` included.

## Settings tab

Lists each source (name; URL, or local folder or "none here"; branch, include,
`mirrors to: community/txt`, list switches when set; note path), duplicate and invalid notes,
and **Add source**. Redraws when a note in `follows/` changes. Folder fields (**Community
folder**, **Fork folder**, the form's **Mirror folder**) suggest vault folders. In the form,
lists are one path per line; **Mirror list** / **Fork list** are Default, On, Off.

- **Add**: **Name** and **Repo URL or folder**. A URL makes a hosted source; an absolute path
  or `file:///` URL a local one. URL valid and not followed; name valid and free, suggested
  when empty (`owner-repo`, or the folder's name). Creates `follows/<name>.md`; a clash is an
  error. A local folder is inspected first ([local.md](community/publictxt-remotes/docs/features/local.md)). A hosted repo isn't
  contacted: a wrong URL shows at the first check. A hosted source can be given a **Local
  copy**.
- **Edit**: rewrites these keys (`processFrontMatter`), keeping others and the body. Name and
  hosted URL are read-only (mirrors and forks record the name; another repo means remove and
  add). A local source's **Folder** and a hosted source's **Local copy** can change; a cleared
  copy is forgotten with its `checkFrom`.
- **Remove**: after confirming, trashes the note (`trashFile`) and forgets its folder here.
  Mirrors, forks and the clone stay.

## Code

Pure: `src/sources/schema.ts`, `repo-url.ts`, `follow-note.ts`, `edit.ts`. Vault:
`src/sources/follows.ts`, `src/settings.ts`, `src/ui/source-modal.ts`.
