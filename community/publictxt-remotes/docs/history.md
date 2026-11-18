---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/history.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: e5316acf3dda4ceaf8087ca58ec36026c1d8f203
mirror_date: 2026-11-17T21:07:37+02:00
---
# History

Past names and dropped designs, for reading old commits. Nothing here is current; existing
vaults are not migrated.

| Was | Now | Why |
|---|---|---|
| Repo `obsidian-publictxt` | `obsidian-publictxt-remotes` | 2026-11-10 |
| GitHub REST adapter | git CLI | Any host, user's auth, no rate limits. Parked at tag `archive/github-adapter` |
| Sources in `data.json` | Follow notes in `follows/` | One public source of truth (ADR-0014) |
| `.remotes/`, `remotesFolder` | `.clones/`, `clonesFolder` | Was taken for the remotes root |
| Remotes root `remotes/<source>/`, forks there | Community folder `community/<source>/`, mirrors there; forks back in `forkFolder` | Named for readers; upstream's copies apart from the user's |
| `watch`, Watched list | Mirrors | A mirror is a watched file the vault also holds |
| `follow_*` keys (planned) | `mirror_*` | "Follow" is for repos; publictxt-pages uses `follow_*` on follow pages |
| Source's fork `folder` | Mirror folder | |
| Fork links left as written when not forked | Link by name | The wiki way |
| Separate one-file and many-file fork modals | One `ForkModal` | |

Design briefs for each feature lived in `docs/plans/` until built; see git history.
