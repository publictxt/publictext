---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/local.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 3adb41345f934d22277574bbdaadbc27a5318dee
mirror_date: 2026-11-17T21:07:37+02:00
---
# Local sources and local copies

## Local sources

A git repo on this computer. Its follow note says `local: true` and holds no path: the folder
is in `localPaths` per computer, so the note can be published and synced. It is cloned and
checked like any source, so a check sees only what is committed.

The plugin writes to the folder (outside the vault) only when asked: **making a repo** and
**snapshot**. Both run the user's git, identity and hooks, one at a time per folder.

### Folder kinds

`inspectLocal()`, from `rev-parse --is-bare-repository --is-inside-git-dir --show-prefix --git-dir`:

| Kind | When | On add | On Snapshot |
|---|---|---|---|
| `missing` | No such folder | Error, not added | Error |
| `unusable` | Inside a `.git` folder, or below a bare repo's top | Error, not added | Error |
| `not-repo` | No repo, or a subfolder of another | Asks: **Make repo and add**, **Add without**, or close | Offers to make a repo |
| `bare` | A bare repo's top | Added; warns if no commits | Error |
| `work` | A working repo's top | Added; warns if nothing committed or `branch` missing | Snapshots |

Warnings are notices; the source is still added.

### Making a repo

`initLocal()`: `init --initial-branch <branch>` (git's default if none), a `.gitignore` unless
one exists (this vault's `configDir`, `.trash/`, `.clones/`, `.DS_Store`), then a snapshot.
Inside another repo it makes a nested one. If it fails partway (hook, no identity) the error
shows and the source isn't added; adding again finds an empty repo and warns.

### Snapshot

**Snapshot** in the check view commits everything changed in the folder; a notice gives the
outcome, and sources are rechecked if it committed. `snapshot()` refuses (`bad-input`) when:

1. The folder isn't the top of a working repo (in a subfolder, `add -A` would stage the
   enclosing repo).
2. HEAD isn't on a branch, or the source names a different `branch`. It never commits to a
   branch that isn't checked out.

Then `status --porcelain -z --untracked-files=all`; if anything, `add -A` and, if the index
differs from HEAD, `commit -m "Snapshot <local ISO time>"`. A failed commit is a `git` error and
leaves changes staged.

## Local copies

A hosted source can also have a working copy here (say, the user's own other vault). Still one
source: the note is the hosted one's and says nothing of the copy, so key, clone, `fork_repo`
and `mirror_repo` stay the hosted URL. The copy's folder in `localPaths` only changes where git
fetches from.

- **Setting it**: **Local copy** in the source form. Must be a working repo's top with a remote
  whose canonical URL is the source's (`hasRemote()`, `copyProblem()`); never made a repo. The
  settings row says `local copy: /path`.
- **Local or remote**: chosen in the check view, saved in `checkFrom`; until chosen, **Local**.
  **Check now**, mirrors and forks use the current choice.
- **Missing folder is an error**: git's own, naming the folder. The network is never used
  instead; the user selects **Remote**.
- **Snapshot**: offered while **Local** is chosen. A copy that's no longer a repo is an error,
  not an offer.
- **Switching can list every file**: copy and remote can be at different commits, so the seen
  commit isn't an ancestor of head. The reason says the two likely differ.
- **Ahead of origin**: a mirror or fork made from an unpushed commit records a commit the host
  lacks until pushed. Not warned of yet.

## Code

Pure: `src/remote/local-repo.ts`, `snapshot.ts`, `fetch-from.ts`. Vault: `src/snapshot/run.ts`.
