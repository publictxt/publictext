---
mirror_remote: pwiki
mirror_path: 01-projects/publictxt/PublicTxt Weighted Links.md
mirror_branch: master
mirror_commit: 44393c174ac3600108d7a58f16dd91a37a4fde5a
mirror_blob: f64eb2da32e3fbee6c4d8349cb82f902fff3dd05
mirror_date: 2026-11-17T21:10:00+02:00
---
#publictxt #idea 

Weighted Links assign a weight to a connection between two pages.

- **Syntax**: 
```
[[PageName]]::weight
```
- Example: `[[PublicTxt]]::89`
- **Meaning**: Forms a weighted edge between the current page and `PageName`.

This enables modelling the strength of relationships directly in the wiki content.
[[Shaped Weighted Network]]