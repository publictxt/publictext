---
project:
  - PublicText
tags:
  - publictxt
  - dotnet
  - dev
  - project
  - dev/app
  - multiplatform
web: https://github.com/publictxt/publictxt.net
status: in progress
mirror_remote: pwiki
mirror_path: 01-projects/publictxt/PublicTxt.Net.md
mirror_branch: master
mirror_commit: 44393c174ac3600108d7a58f16dd91a37a4fde5a
mirror_blob: 8dd7e8ac218b2b6b7fb52353ac0727df8b4c5c01
mirror_date: 2026-11-17T21:10:00+02:00
---
[[publictext.net]]

blazor app, web assembly 
	- open local folder
		- txt wiki folder import 
		- into [[community/pwiki/08-archive/jwiki pages/SQLite]] DB -client side
	- server side
		- connect to external txt
		- filter pull
		- 
# Readme:
PublicTxt.Net is an in-progress .NET implementation of the [PublicTxt](https://github.com/publictxt/publictext) idea, consisting of:

- A core library for reading, writing, and syncing Markdown-based PublicTxt instances to Git repositories.
- Feature based services
- Infrastructure Projects:
    - Git Repository access and management
    - Local Database Options
- Cross-platform clients:
    - Avalonia Desktop App for editing and aggregating knowledge locally.
    - Blazor Web App for web-based access and collaboration.

## Solution Project Structure

- Core
    - PublicTxt.Core
- Features
    - PublicTxt.Wiki
    - PublicTxt.Community
    - PublicTxt.Blog
    - PublicTxt.MetaWeb
- Infrastructure
    - PublicTxt.Git
    - PublicTxt.Data
- Apps
    - PublicTxt.Blazor: Web client for interactive editing and viewing.
    - PublicTxt.Avalonia: Cross-platform desktop interface.
    - PublicTxt.Tools: CLI utilities for batch operations, publishing, and data conversion.

# Notes
## PublicTxt.NET Solution Structure - Draft v1

- **PublicTxt.NET/**
    - PublicTxt.NET.sln
    - **src/**
        - **Core/**
            - **PublicTxt.Core/**
                - PublicTxt.Core.csproj
                - **Models/**
                    - **Content/**
                        - BlogPost.cs
                        - WikiPage.cs
                        - Note.cs
                        - MetaWebContent.cs
                    - **Repository/**
                        - PublicTxtRepository.cs
                        - RepositoryStructure.cs
                    - **Common/**
                        - Tag.cs
                        - Metadata.cs
                        - Link.cs
                - **Interfaces/**
                    - IContentParser.cs
                    - IRepositoryProvider.cs
                    - IContentValidator.cs
                - **Exceptions/**
                    - PublicTxtException.cs
            - **PublicTxt.Syntax/**
                - PublicTxt.Syntax.csproj
                - **Parsers/**
                    - MarkdownParser.cs
                    - MetadataParser.cs
                    - LinkParser.cs
                - **Validators/**
                    - SyntaxValidator.cs
                - **Extensions/**
                    - ObsidianCompatibility.cs
        - **Infrastructure/**
            - **PublicTxt.Git/**
                - PublicTxt.Git.csproj
                - **Services/**
                    - GitRepositoryService.cs
                    - GitCommitService.cs
                    - GitBranchService.cs
                - **Models/**
                    - GitRepository.cs
                    - GitCommit.cs
                    - GitFileChange.cs
                - **Interfaces/**
                    - IGitRepositoryService.cs
                    - IGitCommitService.cs
            - **PublicTxt.Data/**
				- PublicTxt.Data.csproj
				- **Database/**
				    - PublicTxtDbContext.cs
				    - DbInitializer.cs
				- **Repositories/**
				    - ContentRepository.cs
				    - TagRepository.cs
				    - IndexRepository.cs
				- **Interfaces/**
				    - IContentRepository.cs
				    - ITagRepository.cs
				- **Sync/**
				    - GitToDbSyncService.cs
				    - DbToGitSyncService.cs
        - **Features/**
            - **PublicTxt.Wiki/**
                - PublicTxt.Wiki.csproj
                - **Services/**
                    - WikiPageService.cs
                    - WikiLinkService.cs
                - **Models/**
                    - WikiPage.cs
                    - WikiLink.cs
            - **PublicTxt.Blog/**
                - PublicTxt.Blog.csproj
                - **Services/**
                    - BlogPostService.cs
                - **Models/**
                    - BlogPost.cs
            - **PublicTxt.MetaWeb/**
                - PublicTxt.MetaWeb.csproj
                - **Services/**
                    - BookmarkService.cs
                    - AnnotationService.cs
                    - WebPageIndexService.cs
                - **Models/**
                    - Bookmark.cs
                    - Annotation.cs
                    - WebPageNote.cs
            - **PublicTxt.Community/**
                - PublicTxt.Community.csproj
                - **Services/**
                    - DiscussionService.cs
                    - ProfileService.cs
                    - GroupService.cs
                - **Models/**
                    - Discussion.cs
                    - UserProfile.cs
                    - Group.cs
        - **Applications/**
            - **PublicTxt.Web/**
                - PublicTxt.Web.csproj
                - Program.cs
                - **Controllers/**
                - **Views/**
                - **Components/**
                - **wwwroot/**
    - **tests/**
        - **Unit/**
            - **PublicTxt.Core.Tests/**
                - PublicTxt.Core.Tests.csproj
            - **PublicTxt.Git.Tests/**
                - PublicTxt.Git.Tests.csproj
            - **PublicTxt.Syntax.Tests/**
                - PublicTxt.Syntax.Tests.csproj
            - **PublicTxt.Wiki.Tests/**
                - PublicTxt.Wiki.Tests.csproj
        - **Integration/**
            - **PublicTxt.Integration.Tests/**
                - PublicTxt.Integration.Tests.csproj
    - **samples/**
        - **SampleRepository/**
            - **blog/**
            - **wiki/**
            - **metaweb/**
            - **community/**
        - **ConsoleApp/**
            - PublicTxt.ConsoleApp.csproj
    - **docs/**
        - **api/**
        - **guides/**
        - **architecture/**
    - **scripts/**
        - build.ps1
        - setup.ps1
    - .gitignore
    - .editorconfig
    - Directory.Build.props
    - Directory.Build.targets
    - global.json
    - README.md

## Key Design Principles

### 1. Clean Architecture Layers

- **Core**: Domain models, interfaces, and business logic
- **Infrastructure**: External concerns (Git operations, file I/O)
- **Features**: Bounded contexts for different PublicTxt features
- **Applications**: Entry points (Web app, console tools)

### 2. Dependency Flow

- Core projects have no external dependencies
- Infrastructure depends on Core
- Features depend on Core (and optionally Infrastructure)
- Applications depend on all layers as needed

### 3. Modular Design

- Each feature area is a separate project
- Allows for independent development and testing
- Supports future plugin architecture

### 4. Git-First Approach

- `PublicTxt.Git` provides the foundational Git operations
- Other projects build upon this Git foundation
- Maintains separation between Git operations and business logic

## Project Dependencies Overview

```
PublicTxt.Web
├── PublicTxt.Wiki
├── PublicTxt.Blog  
├── PublicTxt.MetaWeb
├── PublicTxt.Community
└── PublicTxt.Git
    └── PublicTxt.Core
        └── PublicTxt.Syntax
```

This structure allows you to:

- Start with `PublicTxt.Core` and `PublicTxt.Git` as foundations
- Add feature projects incrementally
- Test each component independently
- Maintain clean separation of concerns
- Support future extensibility


## Solution Structure in detail

- **PublicTxt.NET/**
    - PublicTxt.NET.sln
    - **src/**
        - **Core/**
            - **PublicTxt.Core/**
                - PublicTxt.Core.csproj
                - **Models/**
                    - **Content/**
                        - BlogPost.cs
                        - WikiPage.cs
                        - Note.cs
                        - MetaWebContent.cs
                    - **Repository/**
                        - PublicTxtRepository.cs
                        - RepositoryStructure.cs
                    - **Common/**
                        - Tag.cs
                        - Metadata.cs
                        - Link.cs
                - **Interfaces/**
                    - IContentParser.cs
                    - IRepositoryProvider.cs
                    - IContentValidator.cs
                - **Exceptions/**
                    - PublicTxtException.cs
            - **PublicTxt.Syntax/**
                - PublicTxt.Syntax.csproj
                - **Parsers/**
                    - MarkdownParser.cs
                    - MetadataParser.cs
                    - LinkParser.cs
                    - FrontMatterParser.cs
                - **Validators/**
                    - SyntaxValidator.cs
                - **Extensions/**
                    - ObsidianCompatibility.cs
        - **Infrastructure/**
            - **PublicTxt.Git/**
                - PublicTxt.Git.csproj
                - **Services/**
                    - GitRepositoryService.cs
                    - GitCommitService.cs
                    - GitBranchService.cs
                    - GitSyncService.cs
                - **Models/**
                    - GitRepository.cs
                    - GitCommit.cs
                    - GitFileChange.cs
                - **Interfaces/**
                    - IGitRepositoryService.cs
                    - IGitCommitService.cs
                    - IGitSyncService.cs
            - **PublicTxt.Data/**
                - PublicTxt.Data.csproj
                - **Database/**
                    - PublicTxtDbContext.cs
                    - DbInitializer.cs
                - **Repositories/**
                    - ContentRepository.cs
                    - TagRepository.cs
                    - IndexRepository.cs
                - **Interfaces/**
                    - IContentRepository.cs
                    - ITagRepository.cs
                - **Sync/**
                    - GitToDbSyncService.cs
                    - DbToGitSyncService.cs
        - **Features/**
            - **PublicTxt.Wiki/**
                - PublicTxt.Wiki.csproj
                - **Services/**
                    - WikiPageService.cs
                    - WikiLinkService.cs
                    - WikiSearchService.cs
                - **Models/**
                    - WikiPage.cs
                    - WikiLink.cs
            - **PublicTxt.Blog/**
                - PublicTxt.Blog.csproj
                - **Services/**
                    - BlogPostService.cs
                    - BlogArchiveService.cs
                - **Models/**
                    - BlogPost.cs
            - **PublicTxt.MetaWeb/**
                - PublicTxt.MetaWeb.csproj
                - **Services/**
                    - BookmarkService.cs
                    - AnnotationService.cs
                    - WebPageIndexService.cs
                - **Models/**
                    - Bookmark.cs
                    - Annotation.cs
                    - WebPageNote.cs
            - **PublicTxt.Community/**
                - PublicTxt.Community.csproj
                - **Services/**
                    - DiscussionService.cs
                    - ProfileService.cs
                    - GroupService.cs
                - **Models/**
                    - Discussion.cs
                    - UserProfile.cs
                    - Group.cs
        - **Apps/**
            - **PublicTxt.Blazor/**
                - PublicTxt.Blazor.csproj
                - Program.cs
                - **Pages/**
                - **Components/**
                - **Services/**
                - **wwwroot/**
            - **PublicTxt.Avalonia/**
                - PublicTxt.Avalonia.csproj
                - Program.cs
                - App.axaml
                - **Views/**
                - **ViewModels/**
                - **Services/**
            - **PublicTxt.Tools/**
                - PublicTxt.Tools.csproj
                - Program.cs
                - **Commands/**
                    - PublishCommand.cs
                    - ImportCommand.cs
                    - ExportCommand.cs
                    - SyncCommand.cs
                - **Utilities/**
    - **tests/**
        - **Unit/**
            - **PublicTxt.Core.Tests/**
                - PublicTxt.Core.Tests.csproj
            - **PublicTxt.Syntax.Tests/**
                - PublicTxt.Syntax.Tests.csproj
            - **PublicTxt.Git.Tests/**
                - PublicTxt.Git.Tests.csproj
            - **PublicTxt.Data.Tests/**
                - PublicTxt.Data.Tests.csproj
            - **PublicTxt.Wiki.Tests/**
                - PublicTxt.Wiki.Tests.csproj
        - **Integration/**
            - **PublicTxt.Integration.Tests/**
                - PublicTxt.Integration.Tests.csproj
    - **samples/**
        - **SampleRepository/**
            - **blog/**
            - **wiki/**
            - **metaweb/**
            - **community/**
    - **docs/**
        - **api/**
        - **guides/**
        - **architecture/**
    - **scripts/**
        - build.ps1
        - setup.ps1
    - .gitignore
    - .editorconfig
    - Directory.Build.props
    - Directory.Build.targets
    - global.json
    - README.md

## Key Components

### Core

- **PublicTxt.Core**: Domain models, interfaces, and core business logic
- **PublicTxt.Syntax**: Markdown parsing, metadata extraction, link parsing, Obsidian compatibility

### Infrastructure

- **PublicTxt.Git**: Git repository operations (clone, commit, push, pull, branch management)
- **PublicTxt.Data**: Local database layer for fast searching/browsing, syncing between Git and DB

### Features

- **PublicTxt.Wiki**: Wiki page management and linking
- **PublicTxt.Blog**: Blog post organization and publishing
- **PublicTxt.MetaWeb**: Social bookmarking and web annotation
- **PublicTxt.Community**: Discussion threads, profiles, and groups

### Apps

- **PublicTxt.Blazor**: Web client for interactive editing and viewing
- **PublicTxt.Avalonia**: Cross-platform desktop interface
- **PublicTxt.Tools**: CLI utilities for batch operations, publishing, and data conversion

## Dependency Flow

```
Apps Layer (Blazor, Avalonia, Tools)
    ↓
Features Layer (Wiki, Blog, MetaWeb, Community)
    ↓
Infrastructure Layer (Git, Data)
    ↓
Core Layer (PublicTxt.Core, PublicTxt.Syntax)
```