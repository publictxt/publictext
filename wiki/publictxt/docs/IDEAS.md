# Ideas (out of scope)

Parked so they don't leak into the spec or the build plan. Each has a trigger for revisiting it. Longer notes are in [ideas/](ideas/).

| Idea | Revisit when |
|---|---|
| Browser extension: MetaWeb overlay on web pages | Aggregation works across at least three real repos |
| Emergent folksonomy from tag co-occurrence | Enough multi-author data exists; it looks broken at 1–3 repos |
| `community/`: profiles, groups, discussion | A group needs a shared repo (spec §13) |
| ActivityPub bridge | Someone outside Git wants to follow a repo |
| W3C Web Annotation export | A consumer needs to interoperate with annotation tools |
| RDF, [weighted links and attributes](ideas/weighted-links-and-attributes.md) | A real query needs typed links |
| TerminusDB or another graph backend | The SQLite projection is too slow for a real query |
| Consensus and reputation | There is multi-author curation to rank |
| Immutable public records or a ledger | A concrete use case needs more than signed commits |
| Avalonia desktop app, Blazor web app ([PublicTxt.net](../projects/PublicTxt.net.md)) | A workflow can't be done with Obsidian, the CLI and the static site (ADR-0008) |
| [PublicTxt Web](ideas/PublicTxt-Web%20idea.md): a hosted service, sites at `yoursite.publictext.net` | Someone who won't use Git wants to contribute (ADR-0008) |
| [Git features](ideas/git-features.md): user views across repos, namespaces, wiki bridges | Follows and forks are used across several real repos |
| Default to `[[wikilinks]]` instead of Markdown links | PublicTxt-Pages converts wikilinks at sync ([plan](https://github.com/publictxt/publictxt-pages/blob/main/docs/plans/wikilinks.md)) and spec §13 q8 is settled |
| Public, semi-public and private content | Someone needs to follow a private repo |
