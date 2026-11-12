---
bookmark: http://fed.wiki.org
tags:
  - wiki
  - social-software
  - knowledge-management
  - decentralised
category:
  - CompSci
  - Tech
fork_remote: txt-local
fork_path: wiki/Computer-Science/Knowledge-Management/FedWiki.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: 6ac72088988c33284aa9d580f075acd1498db7a2
fork_date: 2026-11-12T16:06:48+02:00
---
Federated Wiki (FedWiki) is a decentralized, open-source collaborative knowledge tool created by [Ward Cunningham](wiki/World/People/Ward%20Cunningham.md) in 2011 that lets individual users host their own separate sites and "fork" pages from one another.

## about
- How it works
	- Individual Ownership: Instead of editing a single central database, every user owns and controls their own wiki site.
	- Page Forking: You can copy any page from another user's site onto your own personal site and edit your local copy while keeping automatic attribution back to the original source.
	- Neighborhoods: As you browse, nearby sites form a dynamic "neighborhood" that lets you search and link across different individual servers easily.
- Core Philosophy
	- A Chorus of Voices: FedWiki rejects the single-consensus model of traditional encyclopedias like Wikipedia. It allows different people to share the same information while maintaining their own distinct points of view.
	- Decentralized Sharing: As discussed by some users on Reddit, this design distributes hosting costs and shifts accountability, though opinions vary on whether it solves traditional agreement challenges or simply decentralizes individual biases.
- Technology
	- Node.js: The core platform runs as a Node.js package available via GitHub and npm.
	- Plugins: Pages use paragraph-sized items and support rich content or experimental web plugins.
	- 