---
name: Make an authenticated edit
description: Log in and edit a Fandom wiki page using the MediaWiki Action API's token + clientlogin + edit flow.
api: https://community.fandom.com/api.php
operations: [query, clientlogin, edit]
---

# Make an authenticated edit

Writes require an authenticated session and a CSRF token. Prefer a
**bot password** (Special:BotPasswords) or OAuth over a raw account password.

## Steps

1. **Get a login token** — `action=query&meta=tokens&type=login&format=json`.
   Read `query.tokens.logintoken`.

2. **Authenticate** — POST `action=clientlogin` with `username`, `password`
   (or bot-password credentials), the `logintoken`, and
   `loginreturnurl`. On success the response sets a session cookie; keep it.

3. **Get a CSRF token** — with the session, call
   `action=query&meta=tokens&type=csrf&format=json`. Read `query.tokens.csrftoken`.

4. **Edit** — POST `action=edit` with `title`, `text` (or `appendtext`),
   `summary`, `token=<csrftoken>`, and `format=json&formatversion=2`.
   Use `basetimestamp` + `starttimestamp` to detect edit conflicts, and
   `createonly`/`nocreate` to guard create-vs-update.

## Conventions

- The CSRF token must accompany every write; tokens are session-scoped.
- Set `assert=user` (or `assert=bot`) so the edit fails if the session dropped.
- Honor `maxlag`, `ratelimited`, and Retry-After (see conventions/fandom-conventions.yml).
- There is no Idempotency-Key; safety comes from CSRF tokens + timestamp conflict checks.
