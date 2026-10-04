# Security

## CSRF protection

POST, PUT, PATCH and DELETE requests must come from your own site. Starlite uses Symfony's
[`SameOriginCsrfTokenManager`](https://symfony.com/doc/current/security/csrf.html#stateless-csrf-tokens):
the browser's `Sec-Fetch-Site` header (or `Origin` / `Referer`) must show the request came from this
site, otherwise it gets a **403**.

There's no token, no cookie and no session. Every browser sends these headers with `fetch()`, which
is how Datastar makes its requests, so nothing needs adding to forms or actions.

A route that must accept requests from elsewhere (a webhook, say) can opt out:

```php
$app->route(['POST'], '/webhooks/payments', [WebhookController::class, 'handle'], 'webhook', csrf: false);
```

Verify those requests another way, for example with the provider's signature.

Behind a reverse proxy, set `APP_TRUSTED_PROXIES` so the check sees the public host and scheme.

## Secrets

- Secrets only come from the environment or `.env`, which lives outside the `public/` web root
  and is never committed. `config/app.php` refuses to boot without `APP_SECRET`.
- Vite only inlines variables prefixed `VITE_PUBLIC_` into the bundle, so server variables can't
  reach the frontend.
- Variables passed to `datastar.get(…)` are signed but readable: never pass secrets through them.

## Content

- Markdown is rendered with raw HTML escaped and `javascript:` links removed.
- Twig escapes all output by default.
- JSON-LD is encoded so that `</script>` in any value can't break out of the tag.
- Only whitelisted file types in a post folder are ever served or published.
- SVGs are served with a script-blocking policy in development; in production, treat SVGs from
  others as code.

## Responses

- Every response sends `X-Content-Type-Options: nosniff`, `Referrer-Policy:
  strict-origin-when-cross-origin` and `X-Frame-Options: SAMEORIGIN`.
- Absolute URLs come from `APP_URL` only, so a forged `Host` header can't poison cached pages.
- Production errors are logged, never shown: visitors get a generic error page.

## Deployment

- There's no HTTP endpoint for deployment tasks: cachetool reaches PHP-FPM through its local socket.
- The reload command comes from the server's own configuration; keep `.env` writable only by the
  deploy user.
- The cachetool phar is pinned to a version and checked against its SHA-256 hash.

## Not yet included

A Content Security Policy header is planned (with hashes for the few inline scripts, so pages stay
cacheable). Until then, add one at the web server if you need it; Datastar needs `'unsafe-eval'`.
