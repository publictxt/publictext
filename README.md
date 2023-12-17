# Project Name: Public Text

## Overview
Public Text is an experimental platform combining the power of Git's version control with the dynamic world of public discourse, web browsing, social bookmarking, wikis and blogging. It's designed to enhance how we manage, share, and interact with information in a transparent, collaborative online space.

This repository contains the Public Text template, which is a starting point for creating a new Public Text repository. It includes a basic directory structure, a README template, and a license. The template is designed to be cloned and personalized for your own Public Text repository.

## Key Features of the ecosystem

- **Plain text**: Using Markdown for simple, flexible content creation and editing. Tags and more complex metadata is stored using an open plaintext protocol. 
- **Immutable Public Records**: Leveraging Git for transparent, reliable record-keeping.
- **Consensus and Reputation Mechanisms**: Empowering community-driven content management and peer review.
- **Fluid Control Settings**: Flexibility with public, semi-public, and private content settings.
- **MetaWeb Commons Functionality**: A browser extension that integrates Git repositories that contain discussion and notes about web pages with the web browsing experience.
- **Social Bookmarking and Blogging**: Interactive platforms for content creation and sharing.
- **Desktop and Mobile UI**: Accessible interfaces for diverse devices, enhancing user interaction, content discovery and creation, and community engagement.
- **Web API**: A central conduit for seamless data integration and communication.
- **ActivityPub**: integration with the decentralized social networking protocol.

## Public Text Template

### Setting Up Your Git Repository

- Clone this repository to start a new Public Text repository that can then be used with the browser extension and tooling.

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
