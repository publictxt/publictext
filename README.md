# Project Name: Public Text / PublicTxt (Concept Phase)

## Overview
PublicTxt is a stab at using plaintext based git repos as an interoperability layer and efficient stores of public, community, and semi-private and private content. 
And then hopefully, eventually also a bunch of tooling that works with these git repos to store, share, and organise content/knowledge/community. The idea is to combine the power of Git's version control with strong graphs of connected public content to provide a way for systems to store and interoperate with content for public discourse, web browsing, social bookmarking, wikis and blogging. It aims to enhance how we manage, share, and interact with information in a transparent, collaborative online space.

The experiment is in early stages. 
Links to software repositories for tooling that works work these plaintext repositories to follow.

## Wanted Features of the Ecosystem

- **Plain text**: Using Markdown for simple, flexible content creation and editing. Tags and more complex metadata is stored using an open plaintext protocol.
- **Rich Semantic Syntax**: Using RDF Semantic Edges and Weighted Attributes and Weighted Links/Edges 
- **Compatibility** Plaintext PublicTxt Syntax should be consistent with Obsidian and Obsidian DataView Plugin links and attributes 
- **Feature Rich UI** Making all the other features easy to use, while storing as much data as possible in plaintext with PublicTxt Syntax
- **Desktop and Mobile UI**: Accessible interfaces for diverse devices, enhancing user interaction, content discovery and creation, and community engagement.
- **Fluid Community Controls**: Flexibility with public, semi-public, and private content settings.
- **TerminusDb Backend**: Use richly featured, collaboration focused, versioned Graph Database like TerminusDb to store the Knowledge Graph  in memory for faster use of the graph in ecosystem applications
- **Social Bookmarking and Blogging**: A standard for storing MetaWeb (SideWiki like) data about web resources. This to be used with  Interactive platforms for content creation and sharing.
- **MetaWeb Commons Functionality**: A browser extension that integrates Git repositories that contain discussion and notes about web pages with the web browsing experience.
- **Immutable Public Records**: Leveraging Git commits - possibly in combination with other forms of distributed ledgers - for transparent and reliable record-keeping.
- **Consensus and Reputation Mechanisms**: Empowering community-driven content management and peer review.
- **Web API**: A central conduit for seamless data integration and communication.
- **Interoperability**: Compatibility with other systems and standards, such as ActivityPub and the W3C Web Annotation Data Model.
- **Decentralisation**: A distributed, peer-to-peer network of repositories, with no central authority.
- **Static Web Generation**: Generating static websites from PublicTxt repositories.
- **Data Portability**: Easy import and export of data in standard formats.


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
