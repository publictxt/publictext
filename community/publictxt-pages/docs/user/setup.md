---
mirror_remote: publictxt-pages
mirror_repo: https://github.com/publictxt/publictxt-pages
mirror_path: docs/user/setup.md
mirror_branch: main
mirror_commit: 184a8535c03384a65b94a3ef851e0127c1b99085
mirror_blob: ba88c3afd613a6678a30b87b2c3967744e88dfaa
mirror_date: 2026-11-25T22:44:54+02:00
---
# Setup

From nothing to a live site in about fifteen minutes. You need a GitHub account and
[Obsidian](https://obsidian.md) (or [another editor](other-editors.md)).

- [1. Create the content repo](#1-create-the-content-repo)
- [2. Open it in Obsidian](#2-open-it-in-obsidian)
- [3. Configure Obsidian](#3-configure-obsidian)
- [4. Add some structure](#4-add-some-structure)
- [5. Add the publish workflow](#5-add-the-publish-workflow)
- [6. Turn on GitHub Pages and push](#6-turn-on-github-pages-and-push)
- [7. Commit and push from Obsidian](#7-commit-and-push-from-obsidian)
- [Optional: preview locally](#optional-preview-locally)
- [Optional: custom domain](#optional-custom-domain)
- [Troubleshooting](#troubleshooting)

## 1. Create the content repo

On GitHub, **New repository** — e.g. `my-notes`. Tick *Add a README* so it has a first commit.

- **Public**: free GitHub Pages needs a public repo (private needs a paid plan).
- The site is public either way. Treat everything you push as published, including
  pages you hide (see [Drafts](writing.md#drafts-and-hidden-pages)).

Clone it to your machine (GitHub Desktop, or `git clone https://github.com/<you>/my-notes`).

## 2. Open it in Obsidian

Obsidian → **Open folder as vault** → pick the cloned `my-notes` folder.

The vault *is* the repo: every note is a `.md` file Git can see. Obsidian adds a
`.obsidian/` folder for its settings; the site ignores it. Commit it if you want your
settings on another machine, but keep per-device noise out — add to `.gitignore`:

```gitignore
.obsidian/workspace*.json
.trash/
```

## 3. Configure Obsidian

These keep links working in Obsidian, on GitHub, and on the site.

**Settings → Files and links**

| Setting | Value | Why |
|---|---|---|
| Use [[Wikilinks]] | **Off** | The site reads Markdown links `[text](page.md)`; `[[wikilinks]]` show as plain text |
| New link format | **Relative path to file** | Links resolve the same everywhere, without a vault root |
| Default location for new attachments | **Same folder as current file** (or *In subfolder under current folder*) | The site finds images beside the note, or in a top-level folder like `media/` |
| Automatically update internal links | **On** | Renames don't break links |

**Settings → Editor**

| Setting | Value | Why |
|---|---|---|
| Strict line breaks | **On** | The site joins single line breaks into one paragraph (standard Markdown); this makes Obsidian show what readers will see |

**Settings → Core plugins → Templates** (optional): set *Template folder location* to
`settings/Templates`, then exclude it from the site — see
[Configuration → Leave folders out](configuration.md#leave-folders-out).

Already have notes with `[[wikilinks]]`? Turning the setting off doesn't convert them.
Find them with search (`[[`) and fix the ones that matter; Obsidian writes Markdown
links for every new link from now on.

## 4. Add some structure

Top-level folders become **sections** in the site's sidebar. A typical start:

```txt
my-notes/
  _index.md            home page text
  blog/
    _index.md          the Blog section's intro (optional)
    20261002-hello.md
  wiki/
  notes/
  media/               shared images, audio
  settings/
    site.toml          site settings (optional; see Configuration)
```

`_index.md` is a folder's own page — the home page at the root, a section's intro
inside a folder. Any folder works; these names are just the conventions the default
config orders and styles. Details: [Writing](writing.md).

`CNAME`, `.gitignore` and `.obsidian/` are not published. `README.md` and `LICENSE` are — to keep
them off, list them in [`skipFiles`](configuration.md#leave-files-out).

## 5. Add the publish workflow

Copy [`deploy/publish-to-github-pages.yml`](../../deploy/publish-to-github-pages.yml)
from this repo into your content repo as:

```txt
my-notes/.github/workflows/publish.yml
```

Obsidian hides dot-folders, so create it in your file manager, a terminal, or on GitHub
(**Add file → Create new file**, type the path).

Edit the **Build** step's `env:`:

```yaml
env:
  HUGO_BASEURL: https://<you>.github.io/my-notes/     # trailing slash; your site's address
  HUGO_TITLE: "My notes"
  HUGO_PARAMS_DESCRIPTION: "What this site is about."
  HUGO_PARAMS_EDITURL: ${{ github.server_url }}/${{ github.repository }}/edit/${{ github.ref_name }}/
```

- `HUGO_BASEURL` — `https://<you>.github.io/<repo>/`, or your custom domain.
- `HUGO_PARAMS_EDITURL` — leave as is: it adds an "Improve this page" link to your
  repo. Set to `""` to hide it.
- Leave `fetch-depth: 0` alone: page dates come from Git history.
- `ref: main` builds with the latest publictxt-pages. Pin a tag or commit if you want
  builds that don't change under you.

More settings: [Configuration](configuration.md).

## 6. Turn on GitHub Pages and push

1. Content repo on GitHub → **Settings → Pages → Build and deployment → Source:
   GitHub Actions**.
2. Commit and push everything.
3. **Actions** tab → *Publish site* → wait for green (a minute or two). The
   *deploy* job shows the site URL.

Every push to `main` now republishes. **Actions → Publish site → Run workflow**
rebuilds without a push (e.g. to pick up a publictxt-pages update).

## 7. Commit and push from Obsidian

Pick one:

- **[Obsidian Git](https://github.com/Vinzent03/obsidian-git)** (community plugin) —
  commit and push from inside Obsidian: command palette → *Commit all changes* then
  *Push*, or turn on auto backup at an interval. Simplest day to day.
- **GitHub Desktop** — see changes, write a message, *Push origin*.
- **Terminal** — `git add -A && git commit -m "notes" && git push`.

Pushing is publishing. Auto backup every few minutes means half-written notes go live;
use [`publish: off`](writing.md#drafts-and-hidden-pages) on drafts, or push by hand.

On mobile, Obsidian Git works but is slow on big vaults; Working Copy (iOS) or an
Obsidian Sync + desktop push setup are alternatives.

## Optional: preview locally

See the site before you push. You need:

- **Python 3.11+** (no packages)
- **Hugo v0.162+**, standard edition (extended works too, but isn't needed) —
  `hugo version` to check; distro packages are often too old, use the
  [release binaries](https://github.com/gohugoio/hugo/releases)
- **Pagefind 1.5.2** on `PATH`, or **Node** (the build fetches Pagefind with `npx`)

```bash
git clone https://github.com/publictxt/publictxt-pages
cd publictxt-pages
python scripts/build.py --source ../my-notes          # full build -> public/
python scripts/build.py --source ../my-notes --serve  # preview at http://localhost:1313/
```

Use `python3` where `python` is Python 2. `--serve` doesn't rebuild search, and doesn't
watch your notes: run a full build first for search, and re-run after editing.

No `--source` builds the bundled example — a tour of every feature, worth a look.

## Optional: custom domain

1. At your DNS provider, point the domain at GitHub Pages
   ([GitHub's guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)).
2. Content repo → **Settings → Pages → Custom domain**.
3. Set `HUGO_BASEURL: https://your.domain/` in the workflow.

A `CNAME` file isn't needed with Actions deploys (and the site doesn't publish one).

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Workflow fails at *Deploy* | Pages source isn't *GitHub Actions* (step 6) |
| Styles missing, links go to the wrong path | `HUGO_BASEURL` wrong — must match the URL exactly, with the repo path and trailing slash |
| Every page has the same date | Shallow clone — keep `fetch-depth: 0` |
| A link shows as `[[text]]` | Wikilink — rewrite as `[text](path.md)` (step 3) |
| An image doesn't show | It's not beside the note or in a top-level folder — see [Images](writing.md#images-audio-and-video); the build log warns |
| Search finds nothing locally | `--serve` skips the search index; run a full build first |
| A page is missing | `publish: off`, an excluded folder, or a skipped file name (`skipFiles`) |
| A Templates note appears on the site | Add its folder to `excludeFolders` |

Build warnings (bad category, missing bookmark URL, misplaced image) are in the
*Build* step's log on the Actions run.
