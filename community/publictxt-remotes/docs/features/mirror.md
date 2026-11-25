---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/mirror.md
mirror_branch: master
mirror_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
mirror_blob: e52136af2173701ce54dc77b3124261d0f899178
mirror_date: 2026-11-25T22:45:40+02:00
---
# Mirror

A mirror is upstream's copy of a page in the vault: searchable, with backlinks and graph, opened
in place. Not for editing: updates overwrite it.

## Provenance

`mirror_remote`, `mirror_repo`, `mirror_path`, `mirror_branch`, `mirror_commit`, `mirror_blob`,
`mirror_date`: as their `fork_*` namesakes ([fork.md](fork.md)), for the version held. No
`mirror_seen`: an update overwrites.

A note is a mirror when `mirror_path` and `mirror_blob` are valid, it has a valid
`mirror_remote` or `mirror_repo`, and it isn't a fork (valid `fork_*` keys win). Writing a
mirror removes every `fork_*` key. Keys decide, not the folder: a mirror moved by hand is still
one.

## Where

`<communityFolder>/<folder or remote name>/<upstream path>` (`mirrorFolderOf()`,
`mirrorPath()`): `wiki/cs/git.md` from `txt` → `community/txt/wiki/cs/git.md`. Never asked.

## Mirror

From the check view (**Mirror** on a file, or **Mirror selected**, which clears ticks) at the
head the check saw, or **Mirror a file from a source** (pick source, skipped if one; fetch head;
pick file). Markdown only. Per file (`planMirrors()`):

- Already mirrored anywhere: left alone.
- Its place taken by something else: skipped, reported.
- Otherwise: blob fetched, note created, `mirror_*` keys added. Upstream front matter that isn't
  valid YAML is refused before writing.

One notice (sticky on failure); the note isn't opened.

## Links

Left as upstream wrote them; Obsidian resolves them. The folder has upstream's layout, so `./` and
`../` links go to the mirror, or dangle until it is mirrored; one from the repo root (`/b.md`)
doesn't find it. Any other may land on the user's note, or another source's page, of that name:
the wiki way, unguarded. A link by path to a page forked away dangles.

## Update

**Update** on a file, or **Update mirrors (n)**, overwrites with upstream at the head the check
saw and rewrites `mirror_*`. No compare. Its mirrors are looked up again, so one moved by hand
since the check is updated where it is.

A mirror isn't overwritten unasked (`mirrorState()`) when it is edited
([fork-update.md](fork-update.md)), or when that can't be told (its version can't be read).
**Update** asks, saying which, and suggests forking; **Update mirrors** skips and names it. A note
that changes while it is being updated is left and reported. A file whose mirrors are all gone
leaves the list.

## Stop

**Stop** on a mirrored file, **Remove** on one gone upstream, or **Stop mirroring this note**:
after confirming, the mirror is trashed, with any edits made to it: the question says so, and
suggests forking. A page mirrored twice stops with both notes, which it names. A gone mirror stays
listed until removed or forked.

## Code

Pure: `src/mirror/mirror.ts`, `update.ts`, `src/copy/copy.ts`, `edited.ts`,
`src/sources/fork-meta.ts`. Vault: `src/mirror/run.ts`, `update-run.ts`, `src/copy/run.ts`, `src/sources/forks.ts`.
