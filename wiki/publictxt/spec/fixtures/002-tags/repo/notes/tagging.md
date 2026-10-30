---
tags: [PublicTxt, "#Obsidian"]
---
# Tag edge cases

Inline #dotnet and #DotNet (duplicate by match) and #publictxt (duplicate of front matter).
Nested #lang/csharp works. Unicode #café works. Underscores #snake_case work.
Numeric #2026 is not a tag. Neither is a#b mid-word.

## Not a tag

A URL fragment https://example.com/page#section is not a tag.

`#inline-code` is ignored.

```
#fenced is ignored
```

Trailing punctuation is dropped: #end.
