# Weighted links and attributes

Parked ([IDEAS.md](../IDEAS.md): "RDF, weighted links and attributes"). Not in the spec. Written down so the syntax isn't reinvented when a real query needs it.

## Weighted links

A weight on a link makes it a weighted edge between the page it's in and the page it names, so the strength of a relationship lives in the text.

```md
[[PublicTxt]]::89
```

## Weighted attributes

The same `::` weight on an attribute, in the style of Dataview's inline fields:

```md
[idea::90] [difficulty::30]
```

`[idea::90]` is an edge from this page to the *idea* attribute, with weight 90.

### Attribute pages

An attribute is a page too, and that page says how its weights read:

- the range: `min::0`, `max::100`
- which end is best: `best::100`
- what the number is: a weight, as opposed to an amount, a percentage, a distance, a time or a count of occurrences
- related attributes

## Before it goes in

- Spec §13 q10 leaves Dataview inline fields (`key:: value`) unparsed in 0.1. A weighted attribute is one, so the two need settling together.
- `[[Page]]::90` is a wikilink followed by text. A Reader that doesn't know the syntax shows `::90` as text, which is the plain-text fallback this wants.
- [Reputation](../spec/publictxt-spec.md) (spec §13 q17) is a weight on a repo; a link weight is a weight on an edge. One vocabulary for both would help.
