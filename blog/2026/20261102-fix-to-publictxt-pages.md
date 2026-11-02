# Fix to publictxt-pages: search on sites served from a subpath
#publictxt #bugfix #search

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

- I haven't built the site locally with both baseURLs. After the next publish, check that `data-base` on the live page reads `/publictext/`.
- On a multilingual site, `site.Home.RelPermalink` would include the language prefix (e.g. `/fr/`), which would point at the wrong place for the index. This site is single-language, so it doesn't apply, but it's worth knowing if that changes.
- The "index not built" message still appears for any load failure. Showing the underlying error, or the URL that was tried, would make the next case quicker to diagnose.

## Getting the fix live

The publish workflow checks out publictxt-pages `main` at build time, so no change is needed in this repo. Re-run the "Publish site" workflow, or push any commit here.
