# 0007. One document model; features are document types

- **Status:** Proposed
- **Date:** 2026-10-13

## Context

- The current layout has 12 projects, including separate Wiki, Blog, Community and MetaWeb modules, before any vertical slice has shipped.
- A wiki page, blog post, note and bookmark share everything structural: path, front matter, tags and links. They differ only in a few validation rules.

## Decision

- One `Document` model with a `type` (spec §10). Type-specific rules are small rule modules in Core.
- Start with three projects:
  - `PublicTxt.Core`: model, parser and rules; pure, no IO
  - `PublicTxt.Store`: the git adapter (ADR-0004) and the SQLite projection
  - `PublicTxt.Cli`: `ptx validate | publish | follow | aggregate`
- Add a project only when a dependency boundary forces it, such as a UI framework.

## Consequences

- + One parser, one index and one test suite. A new type is a manifest entry plus a rule module.
- + Fits a functional core / imperative shell: read → parse → project → render.
- − Core could grow large. Keep each type's rules in its own folder and namespace.
