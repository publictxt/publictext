# Project Name: Public Text / PublicTxt

## Overview

PublicTxt is an experiment in using Git repositories as an interoperability layer and online storage for public and community knowledge. By leveraging free Git hosting services and plain text markdown files, it enables individuals and communities to share, curate, and collaborate on content without relying on centralized platforms or paid infrastructure.

The core idea is simple: Desktop and Web applications that sync between local and multiple Git repositories to store and share knowledge, subscribe to repositories and aggregate content from different sources, collaborate through user friendly abstractions over Git's branching and merging workflows, and publish everything to the web using static site generation.

The experiment is in early stages. Links to software repositories for tooling that works with these plaintext repositories to follow.

## Linked Repos

- [WikiTool](https://github.com/jaysen/WikiTool) In-progress. Multiplatform .NET Tool for handling multiple wikis
  - Core Wiki Libraries
  - Converts between Wiki formats and Wiki-Syntax
  - Copy between wikis based on search, tag expressions (tag1 + tag2 - tag3), attributes.
- [PublicTxt.net](https://github.com/publictxt/publictxt.net) An in-progress .NET implementation with:
  - Core libraries, and feature based services  
  - Git and Database Infrastructure projects  
  - Avalonia Desktop & Blazor Web Apps

## Core Features

- **Zero Cost Online Storage and Web Hosting**: Leverages free Git hosting (GitHub, GitLab, etc.) for all storage and distribution, and for static websites.
- **Static Website Generation**: Simple publish to web flow requiring zero/minimal setup. 
- **Plain text**: Using Markdown for simple, flexible content creation and editing. Tags and more complex metadata is stored using plaintext syntax.
- **Compatibility** Plaintext PublicTxt Syntax should be consistent with Obsidian and Obsidian DataView Plugin links and attributes 
- **Desktop UI**: Native applications that sync with Git repositories and pull repository data into local database for fast searching and browsing. User interfaces for content creation, organisation, discovery, search, aggregation, and community engagement. 
- **User Friendly Git Operations**: Desktop and web applications hide the complexity of Git operations
- **Selective Syncing with Multiple Repositories**: The possibility to aggregate from and sync to multiple repos - with syncing rules per topic/repo/type.  
- **Feature Rich UI** Making all the other features easy to use, while storing as much data as possible in plaintext with PublicTxt Syntax, synced and transferred to database for use by applications.
- **Fluid Community Controls**: Flexibility with public, semi-public, and private content settings.
- **Social Bookmarking and  Annotation**: A standard for storing MetaWeb (SideWiki like) data about web resources, using W3C Annotation standards.
- **GIT Topic Branches**: Using different branches to allow following certain aspects of a repo. These use shared naming standards.

## Use Cases

- **Personal and Community Knowledge Management**: Maintain your own markdown-based knowledge base with version control
- **Community Wikis and Content Management**: Collaborative knowledge bases maintained by multiple contributors
- **Social Bookmarking**: Share and annotate web resources with commentary and discussion
- **Distributed Blogging**: Publish blog posts that others can subscribe to and aggregate
- **Research Collaboration**: Share research notes and findings with selective access control
- **Learning Communities**: Build shared knowledge bases around specific topics or fields

## Future Ideas/Possibilities

- **MetaWeb Commons Functionality**: A browser extension that integrates Git repositories that contain discussion and notes about web pages with the web browsing experience.
- **ActivityPub**: integration with the decentralized social networking protocol.
- **Immutable Public Records**: Leveraging Git commits - possibly in combination with other forms of distributed ledgers - for transparent and reliable record-keeping.
- **Rich Semantic Syntax**: Using RDF Semantic Edges and Weighted Attributes and Weighted Links/Edges 
- **TerminusDb Backend**: Use richly featured, collaboration focused, versioned Graph Database like TerminusDb to store the Knowledge Graph  in memory for faster use of the graph in ecosystem applications
- **Consensus and Reputation Mechanisms**: Empowering community-driven content management and peer review.

## Public Text Template

### Using this Repository to work or play with the format

- This repo should also function as a template of the format. Although it also holds a wiki and other content about the entire PublicTxt idea. If  
- This repository should be able to be used with custom software, like the hoped for browser extension and other tooling.
- Suggest changes via pull requests, issues, comments, or by forking the repository and creating your own version of the template.

### Directory Structure

The directory structure for a Public Text repository is as follows:

- **blog**: Contains blog posts
  - **year** (eg 2023)
    - **month** (eg 01)
      - **day** (eg 01)
        - date.md: (eg 20231217.md) Contains the blog post
        - title.md: other posts for the day
- **wiki**: Contains wiki pages
- **notes**: Contains notes
- **media**: Contains media files
- **metaweb**: Contains bookmarks, annotations and notes about web pages
  - **sites**: (eg www.example.com.md) folder containing all web pages with data for that site
    - [modified url].md: (eg www.example.com-folder-page.md)
  - **bookmarks**
  - **notes**
  - **annotations**
  - **indexes**: Contains indexes for web pages
  - **tags**: tag indexes for metaweb content
- **tags**: Contains tag indexes for the entire repository
- **indexes**: Contains indexes for the entire repository
- **community**: Contains community content
  - **discussion**: Contains discussion threads
  - **profiles**: Contains user profiles
  - **groups**: Contains group profiles
  - **settings**: Contains settings files
  - **indexes**: Contains indexes for community content
  - **tags**: Contains tag indexes for community content
- **settings**: Contains settings files



## License
- GPLv3: https://www.gnu.org/licenses/gpl-3.0.en.html 
- see [LICENSE](LICENSE) for more information

## Contributing
- See [CONTRIBUTING.md](CONTRIBUTING.md) for information on contributing to this project.
