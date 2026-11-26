---
aliases:
  - Learning Erlang by building a Discovery Node for PublicTxt
tags:
  - publictxt
  - erlang
  - publictxt-web
  - design
  - dev/planned
  - sticky
rating: "4"
---
## Why Erlang, why now

[PublicTxt](../../wiki/publictxt/PublicTxt.md) v1 (personal storage and publishing) is now working for me day to day, through [my Obsidian Plugins for PublicTxt](../../wiki/publictxt/projects/obsidian-plugins/Obsidian%20Plugins%20for%20PublicTxt.md) and [PublicTxt-Pages](../../wiki/publictxt/projects/publictxt-pages/PublicTxt-Pages.md) for publishing. Community aggregation has already started: the [Obsidian-PublicTxt-Remotes Plugin](../../wiki/publictxt/projects/obsidian-plugins/publictxt-remotes/Obsidian-PublicTxt-Remotes.md) pulls in content from subscribed repos on one machine, and community features in [PublicTxt-Pages](../../wiki/publictxt/projects/publictxt-pages/PublicTxt-Pages.md) are coming along. The next step is a hosted aggregator that follows repos at scale.

That's also my excuse to learn [Erlang](../../wiki/Computer-Science/Languages/Erlang/Erlang.md) properly, not through toy exercises but through a project that needs what Erlang is good at: lots of small, independent, long-lived things that sometimes fail. I started [PublicTxt](../../wiki/publictxt/PublicTxt.md) in [.NET](../../wiki/publictxt/projects/PublicTxt.net.md) because it was the tool at hand. The aggregator's shape is very Erlang.

## The problem

PublicTxt is a decentralised, Git-backed knowledge platform. Each person keeps their notes and bookmarks as plain Markdown in their own Git repo. [CuratedCommons](../../wiki/publictxt/projects/CuratedCommons.md) is the curation layer on top: social bookmarking across many repos.

The stack is free. But following hundreds or thousands of repos, keeping an index fresh and pushing updates to readers is work someone has to run. That's where [PublicTxt-Web](../../wiki/publictxt/projects/PublicTxt-Web.md) comes in: a hosted service that aggregates, indexes and, potentially, offers convenient paid hosting, without locking anyone in.

## The shape of the solution

In Erlang, every followed repo gets its own process. Each one wakes on a timer, fetches, parses what changed and reports back. If a repo is broken, slow or gone, only its own process crashes, and a supervisor restarts it. Everything else carries on. In most stacks you'd hand-build that isolation; in Erlang it's the default.

The pieces:

- **Followers**, one per repo, under a supervisor that restarts them independently.
- **An index process**: the only writer to a fast in-memory table. Readers query the table directly; only writes queue.
- **A committer** that periodically snapshots the index into a public Git repo.
- **An API and feed layer** for queries and live updates.

## Lessons along the way

A few ideas clicked while designing this:

- **Crashes stay local.** Processes share no memory. Links tie fates together on purpose; monitors just let you watch. A room shouldn't die because one user did.
- **Single writer beats locks.** One process owning the index means updates are applied one at a time, so they can't conflict. And because an in-memory table dies with its owner, you give it to a stable process, never a fragile one.
- **Duplicates are a key choice.** Keying bookmarks on a hash of the normalised URL makes repeat updates harmless overwrites, and merging the same resource from many repos is exactly where shared tags start to emerge.
- **Mailboxes have no ceiling.** If followers fire and forget, a busy index drowns. Making them wait for a reply caps each follower at one message in flight. That's [backpressure](../../wiki/Computer-Science/Languages/Erlang/Erlang%20backpressure.md): slowness flows back to the producers.

## The twist: the index as a discovery node

The in-memory index is for speed. But committing a snapshot to a public Git repo turns it into something more: a free, persistent discovery layer. Anyone can clone it and find PublicTxt repos without my server existing at all.

That makes each server just one node. Nodes can follow each other's index repos and trust lists, and discovery spreads without a central registry.

## Consent and trust

Inclusion is opt-in, plus a list of already-public repos I trust. Public isn't quite the same as consent to aggregation, so nodes should honour an opt-out flag and respect licences. Indexing links and tags is a lighter touch than republishing content.

The trust list itself lives in Git, so others can follow my curation, which makes it [CuratedCommons](../../wiki/publictxt/projects/CuratedCommons.md) for repos. To stop trust from sprawling, following is limited to a hop or two.

## What's next

Theory's done. The first piece of code is a single follower: one [gen_server](Erlang%20gen_server) polling one repo on a timer. Small, but it's the seed of the whole thing.
