# 0011. Follow and fork from an Obsidian plugin, desktop-only, over the git CLI

- **Status:** Proposed
- **Date:** 2026-11-03, recording the decision taken in obsidian-publictxt

## Context

- v1 success is pulling selected pages from other people's repos into a local view. Nothing does that yet.
- ADR-0008 names an Obsidian plugin as the first candidate when Obsidian, the CLI and the static site can't do a workflow. Following and forking is that workflow, and `ptx` doesn't exist yet.
- obsidian-publictxt-facets browses a vault and stays offline by design. Pulling needs network and writes to the vault, so it is a separate plugin: obsidian-publictxt.
- An Obsidian plugin can talk to remotes through a host's HTTPS API (any platform, one adapter per host, rate limits, token auth) or the git CLI (desktop only, any host, the user's own auth). A GitHub REST adapter was built and tested, then parked at tag `archive/github-adapter`.

## Decision

- obsidian-publictxt follows repos and forks pages from them. First use case: follow, see what changed since I last looked, fork a page with provenance, see which forks changed upstream.
- Desktop-only, using the git CLI, as ADR-0004 does for .NET.
- One bare partial clone per followed repo (`clone --bare --filter=blob:none`) in a vault folder (`remotes/` by default). Bare clones have no working files, so no remote note shows up as a vault note.
- Follow whole repos; `include` folder prefixes narrow what is shown, not what is fetched (ADR-0005, spec §3).
- Change detection is by git blob SHA: content-addressed and the same on every host.
- Followed repos live in plugin data for now. Field names match manifest `follows` (spec §3), so moving them there is mechanical.

## Consequences

- + Any git host, the user's SSH keys and credential helpers, no API rate limits, full history, and `git merge-file` for updating forks later.
- + The same git-as-the-API approach on both implementations, so behaviour can be reproduced from a terminal.
- + Spec gains a canonical repo URL (§3.1), `follows` `branch` and `include` (§3), and fork provenance keys (§5.2), all proposed.
- − No mobile. A later phase adds a second remote host over HTTPS from the parked adapter; that brings back auth, rate limits and a merge that doesn't use git. `isDesktopOnly` can go from true to false later without taking anything from anyone.
- − The follow list lives in private plugin data, not in public `publictxt.yaml` as ADR-0005 intends, until the move is made.
- ! Clones sit inside the vault. Vault git repos and Obsidian Sync would carry them unless `remotes/` is ignored, or the folder is renamed `.remotes/`.
