---
tags: [publictxt, docs]
---
# Related projects and tools

Projects that share part of PublicTxt's aim, and the software it builds on.

## Similar projects

- **[Agora](Agora.md)** - a free knowledge commons by Flancian. People keep notes in their own tools and Git repos (Markdown, Mycomarkup or Org mode) and register the repo with an Agora. Each note joins the *node* that matches its file name, so following a `[[wikilink]]` shows every contributor's page of that name. Social posts that contain a wikilink, from Mastodon or Bluesky, join nodes too. Closest to PublicTxt's aggregation; the difference is that an Agora aggregates centrally by name, while PublicTxt aggregates locally by follow, one reader at a time.
- **[Solid](https://solidproject.org/)** — Tim Berners-Lee's personal online data stores (*Pods*) that apps read and write with the owner's permission, on open standards. Shares PublicTxt's "your data, your choice" aim; PublicTxt uses public Git repos where Solid uses Pods. There may be room for overlap.
- **[FedWiki](../../Computer-Science/Knowledge-Management/FedWiki.md)** — Ward Cunningham's federated wiki, where pages are forked between sites. The fork-with-provenance model ([ADR-0012](docs/adr/0012-fork-provenance-in-front-matter.md)) is close to it.

## What PublicTxt builds on

| Tool | Used for |
|---|---|
| [Git](https://git-scm.com/) and free Git hosting | Storage, history, distribution, publishing ([ADR-0003](docs/adr/0003-git-is-the-source-of-truth.md), [ADR-0004](docs/adr/0004-use-git-cli.md)) |
| [Obsidian](https://obsidian.md) | The editor ([ADR-0008](docs/adr/0008-obsidian-is-the-editor.md)); a PublicTxt repository is an Obsidian vault. Plugins: [community.obsidian.md](https://community.obsidian.md) |
| [Obsidian Git](https://github.com/Vinzent03/obsidian-git) | Committing and pushing a vault from Obsidian |
| [Obsidian Web Clipper](https://obsidian.md/clipper) | Saving web pages as Markdown notes: a quick way to make a bookmark page ([spec §8](docs/spec/publictxt-spec.md)) |
| [Hugo](https://gohugo.io/) | [PublicTxt-Pages](projects/publictxt-pages/PublicTxt-Pages.md)' static site build |
| [Pagefind](https://pagefind.app/) | Static full-text search for PublicTxt-Pages, with no server |

## Possible later

- [ActivityPub](../../Computer-Science/ActivityPub.md) — following a repo from the Fediverse ([IDEAS](docs/IDEAS.md))
- [TerminusDB](https://terminusdb.com/) — a versioned graph database for a knowledge-graph backend ([IDEAS](docs/IDEAS.md))
- [W3C Web Annotation](https://www.w3.org/TR/annotation-model/) — the annotation format behind [CuratedCommons](projects/CuratedCommons.md)
