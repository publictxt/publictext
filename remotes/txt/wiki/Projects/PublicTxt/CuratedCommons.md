---
bookmark: https://github.com/publictxt/curated-commons
category:
  - Project
fork_remote: txt-local
fork_path: wiki/Projects/PublicTxt/CuratedCommons.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: 2e50901a3bdeb6409c34dd02627ca4f35487534e
fork_date: 2026-11-12T16:06:49+02:00
---
_A PublicTxt project post_ - an extension of [Project PublicTxt](Project%20PublicTxt.md) - using human computation to collectively curate the internet and all things.
#publictxt #km #knowledge-management #social-software #social-bookmarking #metaweb

# Curated Commons: open aggregation for the open web

We already have Creative Commons for content itself - a way for creators to share their work and let others remix and build on it. But the conversation that forms around that content, the commentary and curation and collective sense-making, still lives wherever the platform decides to keep it.

Curated Commons is my attempt at fixing that gap. Basically: Creative Commons, but for the curation layer.

## The Problem with Centralized Aggregation

Most aggregation platforms work by controlling a single choke point. They decide what surfaces, what gets amplified, whose voice carries weight, and you consume it through whatever lens the algorithm hands you that day. The people actually doing the work - curators, annotators, the folks making sense of a pile of links - end up generating value for the platform rather than for the community they're serving.

Even on platforms that let you comment, that commentary is stuck there. You can't fork a conversation. You can't follow a curator you like across different sources. You can't take the curation layer and remix it into your own view.

## Curated Commons: Creative Commons for Curation

The idea borrows pretty directly from Creative Commons: open standards, portable content, ownership that sits with the community instead of a platform. So instead of commentary living inside somebody's database, it lives in plain text repos - out in the open, fork-able, subscribe-able.

A few things fall out of that naturally:

- Different communities can run their own curated threads on the same source material, side by side, without stepping on each other.
- You can follow a curator you trust and get their annotations wherever they show up, across repos and sources.
- Curation becomes something you can publish and cite in its own right, not a footnote trapped in someone else's app.
- Anyone can fork a curated collection and build their own interpretation on top, same as forking the underlying work under Creative Commons.

For the actual format, I'm using the W3C Web Annotation standard extended with PublicTxt syntax - plaintext, Git-backed, and it plays nicely with tools people already use, like Obsidian. So a curated commons is about as portable and durable as the content it's annotating.

## Zero-Cost, Open Infrastructure

[Project PublicTxt](Project%20PublicTxt.md)'s whole premise is that the infrastructure shouldn't cost anything, and Curated Commons inherits that directly. Repos sit on free Git hosting - GitHub, GitLab, wherever - no subscription, no API key, nothing gatekeeping who gets to participate. Any community can spin one up for nothing and start publishing their take on the web.

That's not a nice-to-have, it's kind of the whole point. Free and open infrastructure is what keeps curation a community act instead of a commercial one.

## Trust without a gatekeeper

The obvious question with anything decentralized: how do you know whose curation to actually trust?

[[Project PublicTxt]]'s community features lean on Git's own history for this - every annotation, edit, and collection is versioned and attributed, so reputation comes from what someone has actually contributed over time, not a score handed down by an algorithm. Different communities can weigh that however makes sense to them: peer review for research groups, editorial judgment for journalism, consensus for open collectives.

Nobody's deciding whose voice matters from above. The community sorts that out itself.

## The bigger picture

Most platforms extract value by owning the aggregation layer. Curated Commons flips that - the infrastructure belongs to whoever's doing the curating. Open standards mean the collections stay useful no matter what app you're reading them through, and Git-backed storage means they're versioned, forkable, and not going anywhere.

Creative Commons handed ownership of creative work back to creators. I want Curated Commons to do the same thing for the curation layer.

That's the direction PublicTxt is heading - a web where curation is as open as the content it's talking about.

---

_PublicTxt is an open project. Follow along, contribute, or fork the idea._
