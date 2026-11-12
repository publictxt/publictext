---
title: WebAssembly
bookmark: https://webassembly.org/
created: 2026-10-05
summary: WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.
tags:
  - clippings
  - dev/web
  - dev
  - compsci
category:
  - CompSci
fork_remote: txt-local
fork_path: wiki/Computer-Science/Software Dev/WASM.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: 640f4443a99ca00e5700cfa324b343b8ca2edabe
fork_date: 2026-11-12T16:06:48+02:00
---
WebAssembly (abbreviated *Wasm*) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.

Developer reference documentation for Wasm can be found on [MDN's WebAssembly pages](https://developer.mozilla.org/en-US/docs/WebAssembly). The open standards for WebAssembly are developed in a [W3C Community Group](https://www.w3.org/community/webassembly/) (that includes representatives from all major browsers) as well as a [W3C Working Group](https://www.w3.org/wasm/).

### Efficient and fast

The Wasm [stack machine](https://webassembly.github.io/spec/core/exec/index.html) is designed to be encoded in a size- and load-time-efficient [binary format](https://webassembly.github.io/spec/core/binary/index.html). WebAssembly aims to execute at native speed by taking advantage of [common hardware capabilities](https://webassembly.org/docs/portability/#assumptions-for-efficient-execution) available on a wide range of platforms.

### Safe

WebAssembly describes a memory-safe, sandboxed [execution environment](https://webassembly.github.io/spec/core/exec/index.html#linear-memory) that may even be implemented inside existing JavaScript virtual machines. When [embedded in the web](https://webassembly.org/docs/web/), WebAssembly will enforce the same-origin and permissions security policies of the browser.

### Open and debuggable

WebAssembly is designed to be pretty-printed in a [textual format](https://webassembly.github.io/spec/core/text/index.html) for debugging, testing, experimenting, optimizing, learning, teaching, and writing programs by hand. The textual format will be used when [viewing the source](https://webassembly.org/docs/faq/#will-webassembly-support-view-source-on-the-web) of Wasm modules on the web.

### Part of the open web platform

WebAssembly is designed to maintain the versionless, feature-tested, and backwards-compatible [nature of the web](https://webassembly.org/docs/web/). WebAssembly modules will be able to call into and out of the JavaScript context and access browser functionality through the same Web APIs accessible from JavaScript. WebAssembly also supports [non-web](https://webassembly.org/docs/non-web/) embeddings.