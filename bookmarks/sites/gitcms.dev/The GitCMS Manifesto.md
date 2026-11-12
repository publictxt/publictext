---
title: The GitCMS Manifesto
bookmark: https://gitcms.dev/manifesto
created: 2026-09-26
description: Why your content belongs in Git, not locked behind a database.
tags:
  - git
  - knowledge-management
  - social-software
category:
  - Tech
  - CompSci
fork_remote: txt-local
fork_path: bookmarks/sites/gitcms.dev/The GitCMS Manifesto.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: 0c5bada34cbb4e580cb1b9097a9fa80efb0881b7
fork_date: 2026-11-12T16:06:48+02:00
---
Below is the [gitcms.dev](gitcms.dev.md) Manifesto:

# Manifesto
## Your content deserves freedom

Every CMS makes the same promise: "We'll make content easy." Then they lock your words in a proprietary database, charge you to export, and hold your workflow hostage.

We think that's broken.

## Content is code

Markdown files in a Git repository are the most durable, portable, and flexible way to store content. They're plain text. They diff. They merge. They survive every platform migration, every CMS pivot, every startup that shuts down.

Your blog posts, your documentation, your changelogs — they should live in *your* repository, in *your* format, under *your* control.

## Git is the CMS

Git already solves the hard problems: version history, branching, collaboration, rollback, audit trails. Every "feature" a traditional CMS rebuilds on top of a database — Git does natively, and does it better.

What's missing is a good editing experience on top of Git. That's the only problem worth solving.

## The editing gap

Developers are comfortable editing Markdown in their IDE — and now, increasingly, through agentic coding tools like Cursor, Windsurf, and Claude Code. Non-technical teammates aren't comfortable with either. Content creators shouldn't need to learn Git commands or prompt engineering to fix a typo.

GitCMS bridges this gap. A visual, Notion-like editor that reads and writes directly to your repository. Every save is a commit. Every edit is traceable. No sync layer, no webhook glue, no shadow database.

## AI-native by accident

Plain Markdown files in a Git repo aren't just human-readable — they're the ideal format for AI. LLMs can read them, write them, review them, and transform them without any API adapter or proprietary format conversion.

But the tooling hasn't kept up. Static site generators, content pipelines, and editing workflows were built for a world where humans were the only writers. As AI becomes a first-class collaborator — drafting posts, updating changelogs, rewriting docs — the tools around content need to evolve too. Not just the format, but the editing surface, the publishing flow, and the way humans and AI hand work back and forth.

By choosing Git and Markdown, you're not just making a good architectural decision today. You're building on a foundation that's ready for that world.

## What we believe

- **Your content, your repo.** No vendor lock-in. `git clone` is your export button.
- **Git-native, not Git-adjacent.** Every save is a real commit, not a sync job.
- **Config lives with code.** GitCMS keeps repo-level config in `.gitcms/`, right next to your code and content. No dashboard settings that drift from reality.
- **Framework-agnostic.** Astro, Next.js, Hugo, whatever comes next. If it reads Markdown, GitCMS works.
- **Editing should be visual.** Markdown is a storage format, not an editing interface. People deserve a good editor.
- **Simplicity is a feature.** Every abstraction we don't add is a dependency you don't inherit.

## The bet

We're betting that the future of content management is *not* another database. It's the file system you already have, the version control you already use, and an editor that gets out of the way.

If that resonates, [give GitCMS a try](https://gitcms.dev/login).