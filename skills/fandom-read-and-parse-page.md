---
name: Read and render a Fandom page
description: Fetch a Fandom wiki page's wikitext, metadata, and rendered HTML via the MediaWiki Action API.
api: https://community.fandom.com/api.php
operations: [query, parse]
---

# Read and render a Fandom page

## Steps

1. **Page metadata + revisions** — `action=query&prop=revisions`:
   `?action=query&prop=revisions&titles=Luke_Skywalker&rvprop=content|timestamp|ids&rvslots=main&format=json&formatversion=2`
   The current wikitext is under `query.pages[].revisions[0].slots.main.content`.

2. **Rendered HTML** — `action=parse` to render a page or arbitrary wikitext:
   `?action=parse&page=Luke_Skywalker&prop=text|sections|categories&format=json&formatversion=2`
   HTML is in `parse.text`.

3. **Templates / infoboxes** — `action=expandtemplates` expands wikitext, and
   `action=parse&prop=templates` lists transcluded templates.

## Conventions

- Anonymous read; send `format=json&formatversion=2`.
- Use `redirects=1` to resolve redirect titles automatically.
- For many titles, batch with `titles=A|B|C` and page via `continue`.
