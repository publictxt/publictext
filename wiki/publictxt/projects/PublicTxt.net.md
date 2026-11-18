---
bookmark: https://github.com/publictxt/publictxt.net
tags:
  - publictxt
  - docs
---
# PublicTxt.net

In progress. A .NET implementation of PublicTxt: a library that reads, writes and syncs PublicTxt repositories with Git, and the services and clients built on it.

The desktop and web apps are deferred until a workflow can't be done with Obsidian, a CLI and the static site ([ADR-0008](../docs/adr/0008-obsidian-is-the-editor.md)). The library is what the planned `ptx` CLI (validate, publish, follow, aggregate) needs.

## First design (draft, 2025)

Four layers, each depending only on the ones below:

| Layer | Projects | Holds |
|---|---|---|
| Apps | `PublicTxt.Tools`, `PublicTxt.Blazor`, `PublicTxt.Avalonia` | CLI (publish, import, export, sync); web and desktop clients, later |
| Features | `PublicTxt.Wiki`, `PublicTxt.Blog`, `PublicTxt.MetaWeb`, `PublicTxt.Community` | One project per feature: wiki pages and links, blog archive, bookmarks and annotations, discussion, profiles and groups |
| Infrastructure | `PublicTxt.Git`, `PublicTxt.Data` | Git operations (clone, commit, push, pull, branches); a local database for fast search, synced from Git |
| Core | `PublicTxt.Core`, `PublicTxt.Syntax` | Domain models and interfaces; Markdown, front matter and link parsing, validation, Obsidian compatibility |

- Core has no external dependencies.
- Git first: `PublicTxt.Git` is the foundation, kept apart from feature logic.
- Start with Core and Git; add features one project at a time, each tested on its own.

Since then, decisions have replaced it:

- **Three projects to start** ([ADR-0007](../docs/adr/0007-one-document-model.md)): `PublicTxt.Core` (one document model, parser and rules; pure, no IO), `PublicTxt.Store` (the git adapter and the SQLite projection) and `PublicTxt.Cli` (`ptx validate | publish | follow | aggregate`). A project is added only when a dependency boundary forces it, such as a UI framework.
- **The git CLI**, not LibGit2Sharp ([ADR-0004](../docs/adr/0004-use-git-cli.md)).
- **Git is the only source of truth**: the database is a projection keyed by commit, deleted and rebuilt at will, never committed ([ADR-0003](../docs/adr/0003-git-is-the-source-of-truth.md)).
- **The spec is the product**: the .NET code is a reference implementation of the [format specification](../docs/spec/publictxt-spec.md), checked against its fixtures ([ADR-0002](../docs/adr/0002-format-spec-is-the-product.md)).

## Web app idea

A Blazor WebAssembly client that opens a local folder, imports a vault into a client-side SQLite database, and on the server side connects to other repos and pulls them through a filter.

- [Repository](https://github.com/publictxt/publictxt.net)
