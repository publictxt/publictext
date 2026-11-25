---
fork_remote: publictxt-remotes
fork_repo: https://github.com/publictxt/obsidian-publictxt-remotes
fork_path: docs/features/sources.md
fork_branch: master
fork_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
fork_blob: 77a6f7b2ee1f71d3144f99080821e76fb1672820
fork_date: 2026-11-25T23:51:25+02:00
---
# publictxt-remotes plugin - Sources and data

## data.json

| Field | Default | Meaning |
|---|---|---|
| `version` | `1` | Schema version |
| `communityFolder` | `community` | Holds each source's mirror folder (`''`: vault root). publictxt-pages names the same folder in its own settings, set by hand |
| `forkFolder` | `''` | Where forks go when not to their source folder (`''`: vault root) |
| `matchFolders` | `true` | Fork modal opens on **Same folder as in the source** |
| `listMirrors` / `listForks` | `false` | Keep a mirror / fork list in each follow note ([lists.md](publictxt-remotes%20Lists.md)) |
| `clonesFolder` | `.clones` | Holds the clones; never empty |
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
spec §3.2). The body is the user's; the plugin touches only front matter and its own list
sections.

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

- **Add**: **Name**, **Repo URL**, **Local path**; at least one of the last two. A URL makes a
  hosted source, and with a path too, the path is its local copy. A path alone (absolute, or a
  `file:///` URL) makes a local source. Each in the other's field is an error, not moved. URL
  valid and not followed; name valid and free, suggested when empty (`owner-repo`, or without a
  URL the folder's name). Creates `follows/<name>.md`; a clash is an error. A folder is
  inspected first ([local.md](publictxt-remotes%20plugin%20-%20local%20sources.md)). A hosted repo isn't contacted: a wrong URL shows at
  the first check.
- **Edit**: rewrites these keys (`processFrontMatter`), keeping others and the body. Name and
  **Repo URL** are read-only (mirrors and forks record the name; another repo, or a URL for a
  local source, means remove and add). **Local path** can change: required for a local source;
  a hosted source's, cleared, is forgotten with its `checkFrom`.
- **Remove**: after confirming, trashes the note (`trashFile`) and forgets its folder here.
  Mirrors, forks and the clone stay.

## Code

Pure: `src/sources/schema.ts`, `repo-url.ts`, `follow-note.ts`, `edit.ts`. Vault:
`src/sources/follows.ts`, `src/settings.ts`, `src/ui/source-modal.ts`.
