# 0004. Use the git CLI, not LibGit2Sharp

- **Status:** Proposed. Supersedes the LibGit2Sharp choice for PublicTxt.Git.
- **Date:** 2026-10-13

## Context

- The LibGit2Sharp 0.30 binary that ships has no native SSH transport, so remotes are limited to HTTPS. Source: the release notes of a third-party wrapper (CodeLogic.GitHelper on NuGet, June 2026). Confirm against the LibGit2Sharp repo before accepting this ADR.
- The author's setup uses SSH keys with agent forwarding.
- Loading LibGit2Sharp's native binaries in Linux containers was already flagged as a risk.
- Users already have `git` installed and configured with keys, credential helpers and signing.

## Decision

- Implement `IGitRepo`, `IOriginRepo` and `IExternalRepo` as a thin wrapper that runs the `git` process.
- Parse machine-readable output only: `-z`, `--porcelain=v2`, `--format` and `cat-file --batch`.
- Require `git` on PATH, and fail fast with a clear message if it is missing.

## Consequences

- + SSH, credential managers, commit signing, shallow clone and partial clone work with no extra code.
- + Behaviour matches the user's terminal, so problems can be reproduced by hand.
- + Settles the open sync/async question: process IO is async, so the read API is async.
- − Each call pays a process start. Use `cat-file --batch` for bulk reads.
- − A browser (WASM) client can't use it. That isn't needed while the web layer is static (ADR-0008).
- − The adapter's integration tests need a real `git`. Use temporary repos, not mocks.
