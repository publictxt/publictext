---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/plans/wikilinks.md
mirror_branch: main
mirror_commit: 184a8535c03384a65b94a3ef851e0127c1b99085
mirror_blob: a7f8883d1440eec228a35b2fd5c65a7125990559
mirror_date: 2026-11-25T22:44:54+02:00
---
# Plan: Obsidian `[[wikilinks]]` at sync

Status: planned, not started. SPEC.md lists it as TBD.

## Design

Sync rewrites each wikilink into an ordinary relative Markdown link; the existing
Hugo hooks do the rest. Sync never emits a URL (Hugo owns slugs, base path, anchors).

| Source | Sync writes | Handled by |
|---|---|---|
| `[[Note]]`, `[[Note\|alias]]` | `[Note](../path/Note.md)` | Hugo embedded link hook |
| `![[Note]]` | `![](../path/Note.md)` | note-embed.html |
| `![[img.png]]`, `![[a.mp3]]` | `![](../path/img.png)` | render-image.html + attachment-url.html |
| dangling | plain text (alias or name) + warning + report entry | — |

## Resolution rules (Obsidian's "shortest unique path")

- Obsidian writes a link as the shortest path that is unique in the vault, and adds
  folder segments as soon as a same-named note exists elsewhere (`[[Movie/Vertigo]]`
  once a root `[[Vertigo]]` appears). So the rule is **path-suffix match**: the link
  text matches the tail of exactly one file's path (case-insensitive, `.md` optional).
- A unique match resolves. A bare name matching several files is **ambiguous**
  (typically a legacy link from before the collision): resolve deterministically
  (same folder as the linking note, then shallowest path, then alphabetical) and
  list it in the report so the author decides.
- Resolve **vault-wide**, then treat a target that is unpublished, excluded or skipped
  as dangling (never retarget to a different same-named note). Dangling renders as
  the alias/name as text, so a draft's name shows only if no alias was given.
- A folder name alone is dangling (as in Obsidian); no section-index fallback in v1.

## Report page

Sync collects everything needing a decision and writes it out: dangling links,
ambiguities (with the chosen target), links to unpublished/excluded targets,
unresolved attachments, unsupported forms (`#^block`, front-matter wikilinks).
Each entry: source page, the link as written, the outcome, the candidates.

Decided: a wiki-style Markdown page, **never published**. Sync never modifies the
source repo, so it is written to `build/` beside `site.toml` (outside
`build/content/`), not into the vault; the author can open or copy it into the vault.
A printed summary on each sync. Settings-driven (a `params` key names/enables it).
Open: if an in-vault note at a configured path is wanted instead, that breaks the
"never modifies the source" contract; decide before building.
It is the start of a general "link health" page (see Vault index, below).

## Vault index (`scripts/vault.py`)

No reusable index exists today: `sync()`'s `pages`/`indexes` dicts are filled during
the copy loop (incomplete, published only); `hidden_folders()` is a separate
`rglob("*.md")` pass; `index.json` is made by Hugo after sync, published pages only.

New pre-pass: one walk before the copy loop, applying the same skip rules
(`SKIP_DIRS`, `excludeFolders`, `skipFiles`, `publish: off`). Records every file's
path and publish state (front matter read once, kept for the main loop), with
lookup by name and path suffix. **A pure lookup**: what is on disk and whether it is
published. Wikilink policy (tie-break, dangling, aliases) stays in `wikilinks.py`.

Slice 1 builds it and uses it for wikilinks only. Later consumers, each its own
change (they move the golden): `hidden_folders()` reads it; sync warns on relative
`.md` links to unpublished/excluded targets (SPEC: "links dangle") and on missing
attachments; backlinks record resolved edges; the report grows into link health.

## Slices

1. **Vault index + note links.** `scripts/vault.py` (the index) and
   `scripts/wikilinks.py`: `[[Note]]`,
   `[[Note|alias]]`, `[[folder/Note]]`, dangling, report. Runs in `normalise_md`
   **before** `linkify()` (else `[[N#Heading]]` loses its `#Heading` to a tag).
   Tests for resolution; golden refresh, read the diff.
2. **Embeds.** `![[Note]]`, `![[file.ext]]`. Widen attachment-url.html lookup to any
   vault file (not just beside the note / top-level folder).
3. **Fragments.** `[[Note#Heading]]`, `[[#Heading]]`; `#^block` degrades to a page
   link (reported). Check the embedded hook's handling of raw fragments first;
   anchorize in Hugo, not Python, if possible.
4. **Later.** `![[img.png|300]]` sizing; backlinks (the graph comes free); the
   vault-index consumers listed above.

## Size (estimate)

Slices 1-3: ~300-350 lines of source, ~250 of tests, a few doc edits. Slice 1 alone
~200-250 source + ~150 tests (a good first PR); slice 2 ~40 + a ~15-line template
change; slice 3 ~30. Riskiest: slice 1's ordering against `linkify()` and the shared
regex (silently changes tags on every page); slice 2's change to attachment-url.html
(serves every existing image); the resolution edge cases (case folding, optional
`.md`, folders named like files, same-named attachments).

## Constraints to keep

- Share the protected-region alternation with `HASHTAG_RE` (code, inline code,
  links, HTML, URLs) rather than a second copy; mind traps.md "alternation order".
- Percent-encode spaces and `)` in emitted paths.
- Table-escaped `[[X\|alias]]` in the body must work.
- Front matter wikilinks stay unconverted (no YAML parser); report them.
- Always on, no switch.
- Fixture cases: `[[Obsidian]]` (beside `Obsidian.md.md` and folder `obsidian.md/`),
  `[[dates.py]]` (dangles beside `Dates.md`), `[[PublicTxt]]`, `[[publictxt-pages]]`.

## Docs to update with the change

wiki map (`vault.py`, `wikilinks.py`, tests), traps.md (new cross-file pair if any), SPEC.md (out of
TBD), docs/user/, and the fixture prose saying a wikilink "renders as literal text".
