# Environment variables

Set them as real environment variables on servers, or in `.env` (real variables win). `.env.example`
lists them all.

| Variable | Required | Default | Description |
|---|---|---|---|
| `APP_SECRET` | yes | | Signs Datastar template URLs. Generate with `openssl rand -hex 32` |
| `APP_URL` | yes | | Public base URL, no trailing slash, e.g. `https://example.com` |
| `APP_DEBUG` | no | off | `1` = development mode |
| `BLOG_PER_PAGE` | no | `20` | Posts per page on `/blog` |
| `APP_TRUSTED_PROXIES` | no | | Comma-separated IPs/CIDRs of trusted proxies, or `REMOTE_ADDR` |
| `APP_OPCACHE` | no | `cachetool` | How `deploy` refreshes Opcache: `cachetool`, `reload` or `none` |
| `APP_FPM_SOCKET` | no | `/run/php-fpm.sock` | PHP-FPM socket or `host:port`, for cachetool |
| `APP_CACHETOOL` | no | `cachetool` | The cachetool binary |
| `APP_OPCACHE_RELOAD_CMD` | no | | The command for `reload` mode, e.g. `sudo systemctl reload php8.4-fpm` |

Frontend code only ever sees variables prefixed `VITE_PUBLIC_` (none by default).
