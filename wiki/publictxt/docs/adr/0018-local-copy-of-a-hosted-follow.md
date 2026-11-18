# 0018. A hosted follow can have a local copy; it stays one follow

- **Status:** Proposed
- **Date:** 2026-11-13, recording a decision taken in obsidian-publictxt-remotes

## Context

- A repo can be reached two ways at once: its hosted origin, and a working copy on the author's computer, such as their own other vault cloned from it.
- ADR-0015 gave two kinds of follow: hosted (`url`) and local (`local: true`, no URL). The same repo followed both ways is two follows.
- Mirrors and forks name the follow they came through (`fork_remote`, `mirror_remote`), and a document's source and source rating are computed from it (ADR-0016). Two follows of one repo split its pages between two sources, with two ratings, and nothing says they are one repo.
- An author working in the copy wants to see commits there before they are pushed.

## Decision

- One follow per repo. A follow with a `url` is the hosted repo's, whatever else the author's computer holds. Its `url` is its identity: forks and mirrors record it as `fork_repo` and `mirror_repo`.
- A tool may fetch a hosted follow from a copy on this computer instead of the network. Where that copy is belongs to the tool's own settings, per computer, as a local source's folder does (ADR-0015). Nothing about it is written into the follow, a fork or a mirror, so Readers and views need nothing new.
- The copy must be that repo: obsidian-publictxt-remotes accepts a folder only if it is a git repo with a remote whose canonical URL (spec §3.1) is the follow's `url`.
- Where the folder isn't there (another computer, an unplugged drive), the follow is fetched from its `url`.
- `local: true` stays for a repo with no public URL.
- Not chosen: two follows linked by a key (`same_as`). Every reader would have to merge them, and their ratings could disagree.

## Consequences

- (+) One source and one source rating for a repo, on the author's computer and on a published site.
- (+) The follow, and the forks and mirrors made through it, read the same wherever they were made: no path, and the hosted URL throughout.
- (+) Checks see the copy's commits before they are pushed, and Snapshot works on it as on a local source.
- (-) A fork or mirror made from an unpushed commit records a `fork_commit` or `mirror_commit` the host doesn't have yet. A link to the original at that commit is broken until the copy is pushed. Not warned of yet.
- (-) Turning an existing `local: true` follow into a hosted one with a copy is done by hand; forks made before then have no `fork_repo`.
