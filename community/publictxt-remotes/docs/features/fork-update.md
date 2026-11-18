---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/fork-update.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 6315f790dbae473c0d0df0d5aeb72c82506a0053
mirror_date: 2026-11-17T21:07:37+02:00
---
# Fork update

Look at upstream's new version beside the note, then take it, keep it alongside, or wave it
off. No merge.

## Compare

**Compare** (check view: the latest copy, at the check's head) or **Compare this fork with
upstream** (active note; reads head itself; "Nothing new upstream." only when `fork_blob` is
head's blob, so a version merely seen is still compared). Opens the note with the **Upstream**
view (`publictxt-upstream`) to its right, read-only; one Upstream pane is reused.

The view's header: source, branch, the note, then

- **What changed**: "12 lines added, 3 removed since you forked." (`diffStat` from `fork_blob`
  to head), or that the forked version is gone upstream.
- **Edited?** kept current as the note saves: edited / as forked / can't tell (forked blob
  unreadable). Edited (`isEdited()`): the body differs ignoring leading blank lines, line
  endings, and links showing the same words (`matchesExceptLinks()`: converted links are no
  edit); or the parsed front matter differs, `fork_*` aside (parsed, because Obsidian rewrites
  the YAML).

Below: upstream rendered, front matter hidden; **Show source** toggles raw text.

## Actions

| Action | Compared copy | Other copies of the file |
|---|---|---|
| **Overwrite** | Text replaced; `fork_branch`, `fork_commit`, `fork_blob`, `fork_date` rewritten; `fork_seen` removed | `fork_seen` = blob |
| **Write alongside** | `fork_seen` = blob | `fork_seen` = blob |
| **Keep mine** | `fork_seen` = blob | `fork_seen` = blob |

Each leaves every copy settled, so the file leaves the check view and the pane closes. Settled
copies aren't written.

- **Overwrite** asks first if the copy is edited or can't tell. Same name and path. Invalid
  upstream YAML is refused; if keys can't be written the note is restored.
- **Write alongside**: a new fork (`writeFork()`) in the copy's folder named
  `<name> (up-01)`, `(up-02)`, … (an existing ` (up-nn)` dropped first). Opens where the copy
  was; now the latest copy.

After **Overwrite** or **Write alongside**, links may be offered for conversion.

**Gone upstream**: **Detach** removes `fork_*` from every copy after asking; **Keep** sets
`fork_seen: gone` (not listed until the file is back and differs).

**Restart**: the view's state is source key, upstream path, blob, branch, commit and note path;
the blob is read from the clone. If the note is gone or no longer that fork, or the source isn't
followed, it says so and offers nothing.

## Code

Pure: `src/fork/update.ts`; `isSettled()`, `latestFirst()` in `src/check/check.ts`. Vault:
`src/fork/update-run.ts`, `compare-run.ts`. UI: `src/ui/upstream-view.ts`.
