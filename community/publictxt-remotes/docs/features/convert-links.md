---
mirror_remote: publictxt-remotes
mirror_repo: https://github.com/publictxt/obsidian-publictxt-remotes
mirror_path: docs/features/convert-links.md
mirror_branch: master
mirror_commit: cf50c1028184abc438baf56902550868ab47e4d8
mirror_blob: 7fa6bd68c8283228dc104ab4cf4bfef8da5515c1
mirror_date: 2026-11-17T21:07:37+02:00
---
# Convert links

A copy's links were written for upstream: relative to where the file sat there, in upstream's
style. Converting points them at the vault's copies. Sources are Obsidian vaults or plain
Markdown repos: `[[wikilinks]]` and `[text](path)`.

**Which links**: the `links` and `embeds` Obsidian's metadata cache finds in the body, so code,
front matter, web links and plain text are never touched and there's no second parser. Not
handled: Markdown links with a title (`[a](b.md "t")`).

## Resolving a link (forks)

Meaning comes from the source, the target from the vault. A link is read against the source's
files at `fork_commit` / `mirror_commit` (`ls-tree` in the clone, no fetch; when that commit is
gone, the paths of the source's copies stand in). In order:

1. **Already local**: if Obsidian resolves it (`getFirstLinkpathDest`) to a copy from this
   source, that's the target. This makes a second run a no-op. If upstream reads it as several
   files or another one, it's listed to check (**kept on your copy**).
2. **Upstream path** (`upstreamTarget()`): the target without `#…` / `|…`, URL-decoded, each
   candidate tried as is then with `.md`:
   - `./`, `../`: relative to `fork_path`'s folder only; `/`: from the root only;
   - else Markdown link: relative, root, by name; wikilink: root, relative, by name.

   By name: upstream paths equal to or ending in `/<target>`; exactly one resolves. Paths
   climbing out of the repo resolve to nothing. Case-insensitive only when no exact match.
   None: **missing**; several: **several**.
3. **Its copy**: the latest copy (`latestFirst()`) of that path from this source.

| Upstream names | Becomes |
|---|---|
| A file with a copy (latest fork or mirror) | A link to the copy |
| A file already linked to one of its copies | Left on that copy |
| The note itself | A link to the note |
| A note with no copy | A link by name: the shortest end of its path upstream reads back as it (`b`, `cs/b`), so it converts once copied |
| Nothing, or several by name | A link by its own name |

A link by name lands on the vault's note of that name or dangles: the wiki way, deliberately
unguarded. One already in the vault's style and by name stays.

**Writing** (`generateMarkdownLink`, so the vault's link settings apply; their setting isn't
public API, so the style is read by asking Obsidian for a link): `#heading`/`#^block` kept;
what the link shows kept (alias only when the new link would show something else:
`[[cs/algorithms]]` → `[[algorithms]]`, or `[[algorithms 1|algorithms]]`); `!` kept from the
original. Unchanged text isn't counted.

**Left as they are**: **not a note** (image etc.), **no name fits** (rare, Markdown-link vaults),
**cannot be converted** (a name with `| # ^ [ ]`, or words that can't be link text).

## Mirrors

The table doesn't apply. A link upstream reads as one note becomes a full path to where its
mirror is or would be, `<mirror folder>/<upstream path>` (`mirrorLink()`): `[[cs/b]]` in a mirror
from `txt` → `[[community/txt/cs/b]]`. It keeps its form (wikilink or Markdown), what it shows,
subpath and `!`. Why:

- Links between a source's mirrors work; a link to a page not mirrored dangles where its mirror
  would be (Obsidian doesn't match a full path by name), never landing on the user's note of
  that name and misrepresenting the source. It works once mirrored, nothing rewritten.
- Relative links get the full path too: Obsidian reads a bare path from the vault root.
- Not restyled: nobody writes in a mirror, and Obsidian and publictxt-pages read both forms.

Left as written, uncounted and unreported: links to no one note upstream, to non-notes, and
every link in a mirror moved out of its mirror folder (`mirrorRoot()` is the path less the
upstream path). A mirror links only to mirrors: a link to a page only forked dangles. Forking a
mirror moves it and Obsidian updates full-path links to it; the next update of those mirrors
points them back.

## When

- **Commands**: **Convert links in this note** (`convert-links`, active fork or mirror);
  **Convert links in every fork and mirror** (`convert-links-all`).
- **Forks are asked.** After a fork, **Overwrite** or **Write alongside**, the plugin counts
  without writing what would convert in every copy of that source (forking `b` makes links to
  `b` convertible in earlier forks). If any, a sticky notice offers **Convert** for the source.
- **Mirrors are not asked.** After **Mirror** and **Update**/**Update mirrors**, just those
  mirrors are converted against the tree at the head they came from (already read when
  mirroring; read from the clone after an update). No vault scan, no report; a notice only if a
  note changed meanwhile.

Both wait up to 2 s for the metadata cache to see new notes, else skip. A notice says what was
converted, how many to notes not in the vault, what was left, and what to check.

A note is written once (`vault.process`), only if a link changed, and not at all if any cached
link position no longer matches the text ("Changed while converting, try again"). `fork_*` and
`mirror_*` keys are untouched, so `fork_blob` stays the base, and converted links don't count
as edits (`matchesExceptLinks()`): **Overwrite** and **Update** don't ask because of them.

## The report

Each conversion that writes rewrites two sections in the follow note of each source covered,
from every fork of it (never mirrors). Owned like the lists ([lists.md](community/publictxt-remotes/docs/features/lists.md)), written in
the same pass, removed when empty. Public with the note.

```markdown
## Links to fix

Links in forks of this source that converting left as they were. Rewritten each time links are converted.

- [[a]] line 3: `![p](../img/p.png)` not a note

## Links to check

Links in forks of this source that converting made a call on: they may not go where upstream meant. Rewritten each time links are converted.

- [[a]] line 5: `[[dup]]` several by name
- [[z]] line 2: `[[x]]` kept on your copy
```

To check stays listed until the link gets a path by hand. Not listed: case-only matches (nearly
always right) and links by name to notes not in the vault (the wiki way).

## Code

Pure: `src/fork/links.ts` (with `matchesExceptLinks()`), `link-report.ts`.
Vault: `src/fork/links-run.ts`.
