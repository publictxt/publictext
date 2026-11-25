---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/git.md
mirror_branch: master
mirror_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
mirror_blob: 9bf441ff4e3eddffcacf04fc6345ce892de423d1
mirror_date: 2026-11-25T22:45:39+02:00
---
# Git host

`RemoteHost` is all checks, mirrors and forks use. `GitHost` implements it with the git CLI.

**Clones.** One bare partial clone per source: `<clonesFolder>/<host>/<owner>/<repo>.git`, or
`<clonesFolder>/local/<name>-<hash>.git` for a local source (`<hash>`: 32-bit FNV-1a of the key,
so names that clean up alike don't share a clone).

**Where it fetches from** (`fetchFrom()`): a hosted source's URL; a local source's folder, or a
hosted source's local copy when **Local** is chosen, as `fileUrl()` from `localPaths`. Origin is
re-set before each fetch (`remote set-url`), so a moved folder is followed. No folder here: a
`not-found` error.

**Safety.**

- Every command runs as that repo alone (`--git-dir=.`); otherwise git in a non-repo folder
  would walk up and act on the vault's own repo.
- A clone folder that isn't a bare repo (say, emptied by a vault `reset`) is a `git` error
  saying to delete it; it re-clones next check.
- Before each clone or fetch the clones folder gets a `.gitignore` of `*` unless it has one,
  so clones never show in a vault repo.
- No shell, `GIT_TERMINAL_PROMPT=0`, one command at a time per clone (queues shared by every
  host, so check, fork and compare take turns). Five-minute timeout, then a `git` error.

| Method | git |
|---|---|
| (first use) | `clone --bare --filter=blob:none`; `remote.origin.fetch = +refs/heads/*:refs/heads/*` |
| `head` | No `branch`: `ls-remote --symref origin HEAD` for the default. `fetch --prune` unless the commit is already here; `rev-parse` commit and tree |
| `tree` | `ls-tree -r -z`, blobs only (submodules skipped) |
| `changes` | `null` if `base` is unknown or not an ancestor (`cat-file -e`, `merge-base --is-ancestor`); else `diff --name-status -z -M` |
| `blob` | `cat-file blob`, fetched on demand |
| `prefetch` | Several blobs in one `fetch` before a multi-file fork; failure is ignored |
| `diffStat` | `diff --numstat <from> <to>`; `null` if git fails or the diff is binary |

`RemoteError` kinds: `no-git` (git won't start), `git` (a command failed), `not-found` (branch
or folder missing), `bad-input` (not a canonical URL or SHA; snapshot refusals).

## Code

Pure: `src/remote/host.ts`, `git.ts`, `fetch-from.ts`. Node: `src/remote/git-process.ts`. Wiring:
`src/remote/vault-host.ts`.
