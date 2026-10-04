# Configuration reference

Every key of `config/app.php`. See [Configuration](../basics/configuration) for how settings and
environment variables fit together.

| Key | Type | Default in `config/app.php` | Description |
|---|---|---|---|
| `secret` | string | `APP_SECRET` (required) | Signs Datastar template URLs |
| `debug` | bool | `APP_DEBUG === '1'` | Development mode: no caches, drafts visible, exception pages |
| `url` | string | `APP_URL` (required, validated) | Public base URL, no trailing slash. Used for every absolute URL |
| `trusted_proxies` | list | from `APP_TRUSTED_PROXIES` | Reverse proxies whose `X-Forwarded-*` headers are trusted |
| `language` | string | `'en'` | The default language (no URL prefix) |
| `languages` | map | `en`, `it` | `code => ['name' => …, 'locale' => …]`; codes like `en` or `pt-br` |
| `blog.per_page` | int | `BLOG_PER_PAGE` or `20` | Posts per page on `/blog` |
| `public` | map | `[]` | Values page scripts may read through `publicConfig()`; an allowlist, refused if a value contains `APP_SECRET` |
| `csp.enabled` | bool | `true` | Send the Content Security Policy header on pages |
| `csp.report_only` | bool | `false` | Report violations in the browser console instead of blocking |
| `csp.sources` | map | `[]` | Extra sources per directive, e.g. `['frame-src' => ['https://player.vimeo.com']]` |
| `site.name` | string | `'Starlite'` | Site name: titles, Open Graph, feed |
| `site.description` | string | | Default meta description and feed subtitle |
| `site.image` | ?string | `null` | Default share image (`/path` or URL) |
| `site.author` | ?string | `null` | Author for posts and the feed; defaults to the site name |

## Overrides

`Kernel::boot($root, $debug, $overrides)` merges `$overrides` over the file. Two extra keys are
only meant for overrides (mostly in tests):

| Key | Default | |
|---|---|---|
| `content_dir` | `<root>/content` | where the blog's `blog/` folder lives |
| `cache_dir` | `<root>/var/cache` | where compiled caches are written |
