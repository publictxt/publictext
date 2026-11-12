---
category:
  - CompSci
updated: 2026-10-03
fork_remote: txt-local
fork_path: wiki/Computer-Science/Software Dev/Languages/Go language.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: fb24a5870ba10bcd6dbcf3aad1bd3e18dc90b9af
fork_date: 2026-11-12T16:06:48+02:00
---
**Go** (often referred to as **Golang**) is an open-source, high-level, general-purpose programming language that is statically typed and compiled.

It was designed by **Google** engineers Robert Griesemer, Rob Pike, and Ken Thompson. Officially announced in **2009**

Go was built to manage modern software development challenges, offering the blazing performance of low-level languages like C++ paired with the clean readability of high-level languages like [Python](Python.md)

Go's predictable performance makes it well suited for enterprise-grade backend infrastructure

## Core Architecture & Key Features

- **Built-in Concurrency:** Go uses **Goroutines** (extremely lightweight threads managed by the Go runtime) and **Channels** to execute tasks in parallel efficiently without overwhelming system CPU or RAM. 
- **Fast Compilation:** Go compiles directly down to standalone machine executable binaries. This bypasses the need for a virtual machine or interpreter, leading to near-instantaneous build times and faster production execution.
- **Automatic Memory Management:** Features a built-in garbage collector to track and free memory safely, preventing common memory leaks without manual code management.
- **"Batteries Included" Standard Library:** Comes packed with powerful native packages handling network I/O, cryptography, and complex HTTP web servers without forcing reliance on unstable third-party frameworks.
- Indentation, spacing, and other surface-level details of code are automatically standardized by the `gofmt` tool. It uses tabs for indentation and blanks for alignment. Alignment assumes that an editor is using a fixed-width font.

## links
- https://go.dev/
	- https://go.dev/learn/
	- https://go.dev/doc/effective_go - Go idioms
	- https://go.dev/src/ - package sources
- https://en.wikipedia.org/wiki/Go_(programming_language)

#dev #compsci #lang 
