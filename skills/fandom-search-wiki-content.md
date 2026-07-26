---
name: Search a Fandom wiki
description: Find pages on any Fandom wiki using the MediaWiki Action API's OpenSearch and search endpoints.
api: https://community.fandom.com/api.php
operations: [opensearch, query]
---

# Search a Fandom wiki

Every Fandom wiki serves the MediaWiki Action API at `https://<wiki>.fandom.com/api.php`.
Pick the wiki subdomain for your fandom (e.g. `starwars`, `community`).

## Steps

1. **Quick suggestions** — call `action=opensearch` for title autocomplete:
   `?action=opensearch&search=jedi&limit=10&format=json`
   Returns `[query, [titles], [descriptions], [urls]]`.

2. **Full-text search** — call `action=query&list=search` for content matches:
   `?action=query&list=search&srsearch=lightsaber&srlimit=20&format=json&formatversion=2`
   Read hits from `query.search[]` (each has `title`, `pageid`, `snippet`).

3. **Page through results** — if the response includes a `continue` object, pass
   its members back on the next request until `batchcomplete` with no `continue`.

## Conventions

- Always send `format=json&formatversion=2` for clean JSON.
- Reads are anonymous — no token needed.
- Respect `maxlag` and back off on HTTP 429/503 (see conventions/fandom-conventions.yml).
