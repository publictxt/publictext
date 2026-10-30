# 0002. The format spec is the product

- **Status:** Proposed
- **Date:** 2026-10-13

## Context

- PublicTxt's core claim is interoperability: many tools, one format.
- The tools it must interoperate with are written in other languages: Obsidian plugins (TypeScript), Hugo (Go), Pagefind (Rust), a browser extension (JavaScript).
- The current plan defines the format implicitly, through a 12-project .NET solution. Any non-.NET tool would have to reverse-engineer the C# behaviour.

## Decision

- The versioned spec (`spec/publictxt-spec.md`) and its conformance fixtures (`spec/fixtures/`) define the format.
- The .NET code is a reference implementation. Where it disagrees with the fixtures, either the code is wrong or the spec changes first.
- Changing a format rule means changing the spec and a fixture in the same commit.

## Consequences

- + A TypeScript Obsidian plugin or a Hugo preprocessor can claim conformance by passing the same fixtures.
- + The fixtures double as the parser's golden tests.
- − The spec has to be maintained alongside the code.
- − Needs discipline: no format feature goes into code before it is in the spec.
