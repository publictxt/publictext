---
fork_remote: publictxt-remotes
fork_repo: https://github.com/publictxt/obsidian-publictxt-remotes
fork_path: docs/features/check.md
fork_branch: master
fork_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
fork_blob: 132cafa0a3bba4f8700f32a947813867008f43fb
fork_date: 2026-11-25T23:51:23+02:00
---
# publictxt-remotes plugin - check feature

## What a check finds

Per source, against the tracked branch's head:

1. Fetch; read the head commit and the recursive tree (`path → blob`).
2. **What's new**: files changed between `seenCommit` and head, within `include`. When
   `seenCommit` is null or history was rewritten (`changes` is `null`), every file in `include`
   is listed, with the reason.
3. **Mirrors**: each upstream file with a mirror whose `mirror_blob` isn't head's, or that is
   gone. A file mirrored twice (copied by hand) lists the stale ones.
4. **Forks**: each upstream file whose **copies** (forks with that `fork_path`) aren't all
   settled. A copy is **settled** when its `fork_blob` or `fork_seen` is head's blob; for a
   gone file, when `fork_seen` is `gone`.

**Mark as seen** moves `seenCommit` to head. Mirrors and forks are dealt with per file, since
their state is their front matter. A check writes nothing into notes.

Details: a rename is in `include` if either path is. A mirror is listed whatever `include` says,
until updated, forked or removed. Forks compare with the tracked branch whatever their
`fork_branch`. Copies are listed latest first: newest `fork_date`, undated last, ties by path.
A file back after `fork_seen: gone` is listed again if its blob differs from `fork_blob`.

## Check view

**Check sources** (command, ribbon) opens the view in the right sidebar and checks every source
in turn; **Open check view** opens it without checking. The last run is in memory only;
`checkedAt` is saved per source. State is saved from what it is when the run ends, so a source
acknowledged mid-run stays acknowledged. A check at startup waits for the workspace to be ready,
so a half-loaded metadata cache can't make sources look unfollowed.

Per source, a header: remote name (opens the follow note), branch, summary (`New 4 · Mirrors 1 ·
Forks 2`, "Nothing new" or "Error"). It starts open when there's an error or anything new;
once the user opens or closes it, it stays that way until the view closes. In the header too:

- **Local** / **Remote**: hosted sources with a local copy; the one in use marked. Selecting
  one saves `checkFrom` and rechecks that source alone (`checkOne()`). Disabled during a check.
- **Snapshot**: local sources, and local copies while **Local** is chosen ([local.md](publictxt-remotes%20plugin%20-%20local%20sources.md)).

Under an open header, the error or "nothing new", then actions:

- **Mark as seen**, when there is something to acknowledge.
- **Browse all files** / **Hide all files**, always.
- **Update mirrors (n)**, when mirrors changed.
- **Mirror selected (n)** / **Fork selected (n)**, when files are ticked.

Then lists:

- **What's new**: a folder tree, folders first with file counts, then files with status. A
  folder holding only one folder is joined (`wiki/cs/se`). Folders start open, or closed when
  every file is listed; then stay as the user left them. Closed folders aren't drawn.
- **Mirrors changed upstream**: a row per `mirror_path`, then each stale mirror. Changed:
  **Update**. Gone: **Remove**, **Fork**.
- **Forks changed upstream**: a row per `fork_path`, then each copy latest first (the first
  marked "latest" when several). Changed: **Compare** (latest copy). Gone: **Detach**,
  **Keep**.
- **All files (n)**, while browsing: every file at head within `include`, seen or not: the way
  back after **Mark as seen**. Folders start closed. Browsing changes no state and stays on
  across checks.

**File rows** in What's new and All files. Markdown files not removed have a tick box and:

| File | Row has |
|---|---|
| Not mirrored | **Mirror**, **Fork** |
| Mirrored | "mirrored" (opens it), **Stop**, **Fork** (moves the mirror) |
| Forked, unsettled | Also **Compare** |
| Forked, settled | Also "forked" |

**Ticks.** A folder's tick box is ticked, half or clear; selecting it ticks all or clears all. It
leaves out forked files, so ticking a folder never re-forks. Ticks are per file across both
lists, persist like open folders, and don't count files a later check doesn't list.

## Code

Pure: `src/check/check.ts`, `file-tree.ts`, `selection.ts`. Vault: `src/check/run.ts`. UI:
`src/ui/check-view.ts`, `check-source.ts`, `tree.ts`.
