---
fork_remote: txt-local
fork_path: blog/2026/20261102-Fix-PublicTxt-Pages-for-sites-served-from-subpath.md
fork_branch: master
fork_commit: 85f3c01ab408f962ce6ba605e40c96f606e381f9
fork_blob: 899a169d9ac8b6c38c72a8f49941d4938a46dabf
fork_date: 2026-11-12T16:06:48+02:00
---
# Fix to publictxt-pages: search on sites served from a subpath
#publictxt #bugfix #search #publictxt-pages

The search page on the published site showed "The search index has not been built yet", even though the index had been built and deployed.

## What was wrong

Search is [Pagefind](https://pagefind.app/). The browser loads `pagefind/pagefind.js` from the site root, so the page needs to know where that root is. publictxt-pages passes it to `search.js` as a `data-base` attribute on the `<html>` tag.

The attribute was generated with `{{ "/" | relURL }}`. For a site at the root of a domain (`https://publictxt.net/`) that gives `/`, which is correct. The site was being served from `https://publictxt.github.io/publictext/`, and the live page still had `data-base="/"`. So `search.js` requested `/pagefind/pagefind.js`, which doesn't exist, instead of `/publictext/pagefind/pagefind.js`.

The error message made it harder to spot. `search.js` shows "index not built" for any failure to load Pagefind, so a wrong path looked the same as a missing build. The index was there all along: the correct URL returned 200.

## The fix

`data-base` is now `{{ site.Home.RelPermalink }}`, which follows the baseURL:

- `https://publictxt.net/` gives `/`
- `https://publictxt.github.io/publictext/` gives `/publictext/`

`search.js` already normalizes the trailing slash, so both forms work.

The change is one line in `layouts/baseof.html`, in publictxt-pages commit `7569bf5`.

## Caveats

- On a multilingual site, `site.Home.RelPermalink` would include the language prefix (e.g. `/fr/`), which would point at the wrong place for the index. This site is single-language, so it doesn't apply, but it's worth knowing if that changes.
- The "index not built" message still appears for any load failure. Showing the underlying error, or the URL that was tried, would make the next case quicker to diagnose.

## Getting the fix live

The publish workflow checks out publictxt-pages `main` at build time, so no change is needed in this repo. Re-run the "Publish site" workflow, or push any commit here.

**Note: in previously broken sites, its necessary to do a hard refresh in browser to reload the search index.**