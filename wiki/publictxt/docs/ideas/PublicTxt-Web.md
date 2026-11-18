# PublicTxt Web

Parked ([IDEAS.md](../IDEAS.md): web app). A hosted service, possibly paid for, that gives anyone a PublicTxt site at `yoursite.publictext.net` (or `publicwiki.net`) and does in the browser what the tools do today, without the person touching Git.

## What it's for

An open invitation to build public knowledge together:

- **Build on public text.** Collect from other people's public work into your own site.
- **Make it public.** Where source material isn't publicly owned, rewrite it so its substance lives in a public project.
- **Disagree by forking.** Take what you agree with into a site of your own, in a click, and carry on from there.

## What it does

The parts that exist, behind one web UI:

| In the browser | Today |
|---|---|
| Write and edit pages | Obsidian or any Markdown editor |
| Follow repos; mirror and fork their pages | [Obsidian PublicTxt Remotes](../../projects/obsidian-plugins/Obsidian-PublicTxt-Remotes.md) |
| Browse by facets, search | [Obsidian PublicTxt Facets](../../projects/obsidian-plugins/Obsidian-PublicTxt-Facets.md), [PublicTxt-Pages](../../projects/publictxt-pages/PublicTxt-Pages.md) |
| Publish | PublicTxt-Pages on GitHub Pages |

One way to build it ([PublicTxt.net](../../projects/PublicTxt.net.md)): a Blazor WebAssembly client that opens a folder or repo into a client-side SQLite cache for fast search, with a server side that fetches followed repos and pulls them through a filter.

## What must stay true

- **Every site is a Git repo the owner can take away.** The service is never the only copy; clone it and leave, or point a custom domain at it. Git is the source of truth ([ADR-0003](../adr/0003-git-is-the-source-of-truth.md)).
- **Same format.** Pages written on the web and in Obsidian are the same files under the [spec](../spec/publictxt-spec.md); a web user and an Obsidian user follow and fork each other.
- **Paid for convenience, not for content.** The free path (a Git host plus PublicTxt-Pages) keeps working, and keeps doing everything that matters.

## Why not now

Obsidian is the editor and custom clients wait until a workflow can't be done with Obsidian, the CLI and the static site ([ADR-0008](../adr/0008-obsidian-is-the-editor.md)). The likely trigger: someone who won't use Git wants to contribute.

## Open

- Whose Git: the service's own server, or the user's GitHub/GitLab account through OAuth?
- What's paid for: hosting, custom domains, private repos, storage?
- Moderation of hosted sites, since the domain is the service's.
- `publictext.net`, `publicwiki.net`, or both for different kinds of site.
