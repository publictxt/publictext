---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/lists.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 14153587a6250e6258ae77cb22bb4462d0ec6d46
mirror_date: 2026-11-17T21:07:37+02:00
---
# Mirror and fork lists

A follow note can end with links to the notes mirrored and forked from its source.

| List | Heading | Kept when |
|---|---|---|
| Mirror list | `## Mirrored pages` | `list_mirrors: true`, or absent and `listMirrors` on |
| Fork list | `## Forked pages` | `list_forks: true`, or absent and `listForks` on |

```markdown
## Mirrored pages

- [git](../community/txt/wiki/cs/git.md)

## Forked pages

- [human computation](../wiki/human%20computation.md)
```

The plugin owns each section, from its heading to the next `#`/`##` heading or the end (so
`###` inside is replaced); nothing outside is touched. Links come from `generateMarkdownLink`
(the vault's link settings), sorted. An empty list has no section: added at the end with the
first entry, removed with the last. Written together, mirrors come first. A heading that appears
twice is left alone with a notice; the other list is still written. A mirror that is forked
moves between lists in one write.

**When.** Debounced, after a mirror or fork is created, renamed, deleted, changes kind or
source; after a follow note changes; when either setting is turned on. Not before the workspace
is ready (a half-loaded cache would empty lists). A note is written only if a kept list changed;
a list no longer kept is left as is. **Update mirror and fork lists in follow notes**
(`update-page-lists`) syncs every note now, skipping lists turned off in the note, whatever the
settings.

The "Links to fix" and "Links to check" sections ([convert-links.md](community/publictxt-remotes/docs/features/convert-links.md)) are
owned the same way and written in the same pass.

## Code

Pure: `src/fork/fork-list.ts`. Vault: `src/fork/fork-lists.ts`.
