---
fork_remote: pwiki
fork_path: 01-projects/publictxt/projects/Obsidian/Obsidian Plugins vs PublicTxt project.md
fork_branch: master
fork_commit: b140866b0265a17697bb5c8440d599eb874c95cb
fork_blob: 343106d9a7b0cadc35e1e5f69cb81734505096a4
fork_date: 2026-11-17T22:16:46+02:00
---
Q: what about implementing as a set of [Obsidian](../../../Computer-Science/Software/Obsidian/Obsidian.md.md) plugins ? how does that weigh up against the [PublicTxt](../../PublicTxt.md) idea

A: It's a genuinely interesting alternative. Here's how it weighs up:

## What You'd Get for Free

- **Obsidian's entire UI** — editor, graph view, search, tags, backlinks, DataView. You'd skip the Avalonia and Blazor apps entirely, at least for the desktop use case.
- **Existing user base** — Obsidian users already live in Markdown vaults. The install friction drops from "download a new app" to "install a plugin."
- **Git sync already half-solved** — the [Obsidian Git](https://github.com/denolehov/obsidian-git) plugin handles basic push/pull. You'd build _on top of_ that rather than from scratch.
- **Mobile** — Obsidian has iOS/Android apps. You'd get that for free.
- **Community plugins ecosystem** — DataView, Templater, Kanban, etc. all just work alongside yours.

## What You'd Lose or Fight Against

- **Obsidian is not open source.** You're building on a commercial, closed product. If they change the API, deprecate plugin capabilities, or shut down, you have no recourse. The content survives (it's Markdown) but the tooling dies.
- **Plugin API is limited by design.** Obsidian deliberately restricts what plugins can do — especially around networking, file system access outside the vault, and anything that looks like a "second app." Selective aggregation from external repos (your killer feature) is exactly the kind of thing that bumps against those limits.
- **No web client.** Obsidian is desktop-only (the Publish product is separate and paid). A Blazor web client for collaboration has no Obsidian equivalent.
- **Link format lock-in.** Obsidian defaults to `[[wikilinks]]`. You'd be fighting to keep md-wiki syntax as the default, or accepting that the format spec bends to Obsidian's preferences.
- **Two codebases, not one.** If you want PublicTxt to work without Obsidian too (CLI, web, non-Obsidian users), you still need PublicTxt.net. The plugin becomes a third client alongside Blazor and Avalonia, not a replacement.

## The Real Question

The plugin approach answers: _"how do I get a usable UI fast for Obsidian users?"_

It doesn't answer: _"how do I build an open, tool-agnostic ecosystem?"_

If the goal is a working personal tool for yourself and a small audience of Obsidian power users, a plugin is a **much faster path to something usable**. You ship the aggregation feature as an Obsidian plugin and skip months of UI work.

If the goal is an open ecosystem that survives beyond Obsidian, the plugin is at best a first client — useful for traction, but not the foundation.

## My Take

Do both, sequentially. **Build the .NET Core and Git infrastructure first** (that's what you're doing) — that's the format-agnostic foundation that doesn't depend on Obsidian. Then, once the aggregation logic works, an Obsidian plugin becomes a thin UI wrapper over it (or a parallel implementation of just the aggregation piece in TypeScript).

The risk of going plugin-first is that you optimize for Obsidian's constraints and end up with a format that's Obsidian-shaped rather than PublicTxt-shaped.