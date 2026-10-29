# Project Name: Public Text / PublicTxt

From [Readme of the parent project - github.com/publictxt/publictext](https://github.com/publictxt/publictext)

## Overview

PublicTxt is an experiment in using Git repositories as an interoperability layer and online storage for public and community knowledge. By leveraging free Git hosting services and plain text Markdown files, it enables individuals and communities to share, curate, and collaborate on content without relying on centralized platforms or paid infrastructure.

The core idea is simple: desktop and web applications that sync between local working copies and one or more Git repositories, **selectively aggregate content from other people's repositories**, and publish to the web using static site generation - all built on plain Markdown that survives the tooling.

The experiment is in early stages. The format is defined in the [PublicTxt Format Specification](spec/publictxt-spec.md) (v0.1.0-draft).

## What PublicTxt Is (and Isn't), For Now

The differentiator - the thing PublicTxt does that existing tools don't - is **selective aggregation across many Git-hosted repositories**: subscribing to other people's PublicTxt repos and pulling chosen subsets (by tag, path, or filter) into a local aggregate view. Plain-text Markdown in Git, published via static hosting, is the *substrate*; aggregation is the *value*.

Everything else in this document supports that, or is deferred.

### v1 Success Looks Like

> "I can keep my notes and writing in a Git repository, publish them to the web for free via gh-pages or similar, and pull selected pages from other people's PublicTxt repos into a local aggregate view I can browse and search."

If that works and feels good, the project is real. Community features, browser extensions, federation protocols, and graph-database backends are all possible later - but they aren't what v1 ships.

## Project Status

| Component | Status |
|---|---|
| [Obsidian PublicTxt Facet Plugin](#obsidian-publictxt-facet-plugin) | **v1.0 released** |
| [PublicTxt-Pages](#publictxt-pages) | Working - live at [jaysen.co.za](https://jaysen.co.za) |
| [Format Specification](spec/publictxt-spec.md) | v0.1.0-draft |
| [WikiTool](https://github.com/jaysen/WikiTool) | In progress |
| [PublicTxt.net](https://github.com/publictxt/publictxt.net) | In progress |
| [CuratedCommons](https://github.com/publictxt/curated-commons) | Specification stage |

## Linked Repos

### PublicTxt-Pages

[github.com/publictxt/publictxt-pages](https://github.com/publictxt/publictxt-pages) - zero-backend static site for browsing and searching a PublicTxt repository (or any Obsidian / Markdown wiki). Formerly Txt-Hugo.

- **Static and free to host**: builds with Hugo and deploys to GitHub Pages or any static host. No server, no hosted search.
- **Works on plain Obsidian vaults**: a Markdown wiki using relative links builds as a functional site with no conversion step (set Obsidian to relative Markdown links).
- **Full-text search and facets** via [Pagefind](https://pagefind.app/): filter by tags (including tag combinations) and document type.
- **Hashtag support**: inline `#hashtags` are pre-processed (Python) into front matter tags.
- **Content types**: wiki, blog, notes and metaweb bookmarks, resolved from folder layout or front matter `type`.
- **Navigation sidebar**: sections, tag cloud / top tags, and page metadata.
- **URL-friendly filenames** generated at build time.
- Single-repo in v1. Example: [jaysen.co.za](https://jaysen.co.za)

### Obsidian PublicTxt Facet Plugin

[github.com/publictxt/obsidian-publictxt-facets](https://github.com/publictxt/obsidian-publictxt-facets) - **v1.0**. Faceted tag navigation inside Obsidian.

- Search and filter notes by **combinations of tags**.
- Selecting a tag narrows the remaining tag list to tags that co-occur with the current selection, so you can drill down without dead ends.
- Brings the same faceted browsing that PublicTxt-Pages offers on the web into the editing environment.

### WikiTool

[github.com/jaysen/WikiTool](https://github.com/jaysen/WikiTool) - in progress. Multiplatform .NET tool for handling multiple wikis.

- Core wiki libraries.
- Converts between wiki formats and wiki syntaxes (incl. Obsidian ↔ md-wiki).
- Copy between wikis based on search, tag expressions (`tag1 + tag2 - tag3`), attributes.

### PublicTxt.net

[github.com/publictxt/publictxt.net](https://github.com/publictxt/publictxt.net) - in-progress .NET implementation.

- Core libraries and feature-based services.
- Git and database infrastructure projects.
- Avalonia desktop & Blazor web apps (later phases).

### CuratedCommons

[github.com/publictxt/curated-commons](https://github.com/publictxt/curated-commons) - social bookmarking and curation on top of PublicTxt repos.

- Borrows its spirit from Creative Commons: open standards, portable content, community ownership. Commentary and curation live in plain text repositories - publicly accessible, forkable, subscribable - instead of inside a centralized platform.
- See [CuratedCommons page in this repo](wiki/publictxt/CuratedCommons.md).

## Core Features (v1 Focus)

These are the load-bearing features for the v1 vision above.

- **Plain Text First**: Markdown on disk, in the md-wiki dialect (standard `[text](page.md)` links) so content is publishable by any static-site host with no conversion step. Tags and structured metadata use plaintext conventions ([spec](spec/publictxt-spec.md)).
- **Zero-Cost Online Storage and Hosting**: Leverages free Git hosting (GitHub, GitLab, etc.) for storage, distribution, and static websites.
- **Static Website Generation**: [PublicTxt-Pages](#publictxt-pages) - push to a Pages-enabled branch and you're live, with full-text search and tag/type facets.
- **Selective Aggregation from Multiple Repositories**: Subscribe to other PublicTxt repos and pull selected content (by tag, path, or filter) into a local aggregate. **This is the killer feature.**
- **Sync to One or More Origin Remotes**: Bidirectional sync with the instance's own remote(s); a single origin to start, multiple later.
- **Obsidian Compatibility**: Files round-trip with Obsidian - either by configuring Obsidian to use Markdown links, or via WikiTool conversion at the editing edge. PublicTxt syntax aims to be compatible with Obsidian Properties and Dataview attributes.
- **Faceted Tag Navigation in Obsidian**: The [Obsidian PublicTxt Facet Plugin](#obsidian-publictxt-facet-plugin) (v1.0) for drilling into notes by tag combinations while editing.

### Later Phase

- **Desktop & Web UI**: Native applications that hide Git's complexity for everyday content workflows, sync with Git repositories, and pull repository data into a local cache for fast search and browsing. See milestones in [ProjectSpec.md](ProjectSpec.md).

## Use Cases (v1 Focus)

- **Personal Knowledge Management**: Maintain your own Markdown knowledge base with version control and free hosting.
- **Distributed Blogging**: Publish blog posts that others can subscribe to and aggregate.
- **Research and Learning Aggregation**: Pull selected content from peers' repositories into a local view for study, reference, or synthesis.
- **Community Wikis (Lightweight)**: Small-group collaborative Markdown wikis using Git's existing collaboration model - pull requests, branches, forks. Heavier community/moderation features are a later phase.

## Public Text Directory Structure

Default layout (the [spec](spec/publictxt-spec.md) §4 is authoritative; a `publictxt.yaml` manifest can remap folders):

- **wiki/**: Wiki pages
- **blog/**`<yyyy>/<mm>/<yyyymmdd>[-<n>]-<slug>.md`: Blog posts, e.g. `blog/2026/09/20260920-1-title.md`
- **notes/**: Notes
- **media/**: Media files (attachments)
- **metaweb/**`<domain>/<subdomain-or-folder>/<file>.md`: One file per bookmarked web resource. Any document with a `bookmark` front matter attribute is also a bookmark, wherever it lives.
- `publictxt.yaml`: Optional repo manifest
- *community/*: Profiles, groups, discussion (deferred)

Indexes and tag pages are derived data and are not committed; tools generate them at build or view time.

## Future Ideas / Later Phases

These ideas are part of the PublicTxt vision but are explicitly deferred until the v1 aggregation-and-publish story is working and used.

### Community & Social

- **Community Features**: Discussion threads, profiles, groups, moderation. Building a social platform on top of Git is a separate, much larger product than a knowledge tool, and is deferred until the underlying tool has users.
- **Fluid Community Controls**: Public / semi-public / private content settings, with selective access control for collaborative use.
- **Collaborative Wikis with Friendly Merge UX**: Hiding Git's branching/merging complexity for non-technical contributors. v1 expects users comfortable with Git's existing collaboration primitives (PRs, branches, forks).

### MetaWeb / Web Annotation

- **MetaWeb Commons**: A browser extension that integrates Git-stored discussion and notes about web pages into the browsing experience.
- **W3C Web Annotation export**: Export bookmarks and annotations in the W3C Annotation format for interoperability.

### Federation & Protocols

- **ActivityPub**: Integration with the decentralized social networking protocol.
- **Immutable Public Records**: Leveraging Git commits - possibly combined with distributed ledgers - for transparent record-keeping.

### Richer Data Models

- **Rich Semantic Syntax**: RDF semantic edges, weighted attributes, weighted links/edges.
- **TerminusDB Backend**: Versioned graph database to store the knowledge graph in memory for fast graph operations across the ecosystem.
- **Consensus and Reputation Mechanisms**: Community-driven content management and peer review.