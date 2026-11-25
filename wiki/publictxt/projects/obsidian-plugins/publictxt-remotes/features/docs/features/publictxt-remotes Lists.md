---
fork_remote: publictxt-remotes
fork_repo: https://github.com/publictxt/obsidian-publictxt-remotes
fork_path: docs/features/lists.md
fork_branch: master
fork_commit: e7b2a3ec63d65ba1ee06578e69f3b9032b6a5f5b
fork_blob: e7e04e27030934454c73ad32517eaefbc8268292
fork_date: 2026-11-25T23:51:25+02:00
---
# publictxt-remotes plugin - Mirror and fork lists

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

## Code

Pure: `src/fork/fork-list.ts`. Vault: `src/fork/fork-lists.ts`.
