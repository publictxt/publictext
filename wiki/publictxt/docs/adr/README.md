# Architecture decision records

Format and process: [0001](0001-record-architecture-decisions.md).

| ADR | Decision | Status |
|---|---|---|
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions | Proposed |
| [0002](0002-format-spec-is-the-product.md) | The format spec is the product | Proposed |
| [0003](0003-git-is-the-source-of-truth.md) | Git is the only source of truth; derived data is never committed | Proposed |
| [0004](0004-use-git-cli.md) | Use the git CLI, not LibGit2Sharp | Proposed |
| [0005](0005-follow-repos-filter-at-query-time.md) | Follow whole repos, filter at query time; no topic branches | Accepted / Proposed |
| [0006](0006-tags-raw-on-disk.md) | Tags stay raw on disk; normalization and aliases at view time | Accepted |
| [0007](0007-one-document-model.md) | One document model; features are document types | Proposed |
| [0008](0008-obsidian-is-the-editor.md) | Obsidian is the editor; defer custom clients | Proposed |
| [0009](0009-single-author-per-repo.md) | One author per repository | Accepted |
| [0010](0010-bookmark-is-an-attribute.md) | Bookmark is an attribute any document can carry | Accepted / Proposed |
