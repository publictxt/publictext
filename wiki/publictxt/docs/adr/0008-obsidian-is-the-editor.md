# 0008. Obsidian is the editor; defer custom desktop and web clients

- **Status:** Proposed
- **Date:** 2026-10-13

## Context

- The README plans an Avalonia desktop app and a Blazor web app covering content creation, organisation and search.
- Obsidian already handles editing, linking and local search, and Obsidian compatibility is a core constraint.
- What PublicTxt adds is following, aggregation and publishing across repos. None of that needs a new editor.

## Decision

- The v1 clients are the CLI (`ptx`) and the static site (`publictxt-pages`).
- Authors edit in Obsidian or any Markdown editor.
- A desktop UI waits until a real workflow can't be done with Obsidian, the CLI and the static site. The first candidate is then an Obsidian plugin, not a standalone app.

## Consequences

- + A large scope cut. Effort goes into the part only PublicTxt provides.
- + Users keep the tools they already use.
- − An Obsidian plugin is TypeScript, so it needs its own parser. ADR-0002's fixtures keep the two in step.
- − People who don't use Obsidian get only the CLI and the static site at first.
