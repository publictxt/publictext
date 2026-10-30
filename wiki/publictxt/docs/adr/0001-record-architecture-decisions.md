# 0001. Record architecture decisions

- **Status:** Proposed
- **Date:** 2026-10-13

## Context

Decisions so far live in chat history and personal notes. Contributors, and future you, can't see what was decided, why, or what was rejected.

## Decision

- Record significant decisions as ADRs in `docs/adr/`, named `NNNN-title.md`.
- Each ADR has Context, Decision and Consequences sections.
- Status is one of Proposed, Accepted, Rejected or Superseded by NNNN.
- An Accepted ADR is not edited. A change of mind is a new ADR that supersedes it.

## Consequences

- + Decisions become reviewable in pull requests.
- + "Why is it like this?" has an answer in the repo.
- − A small overhead per decision. Keep each ADR under a page.
