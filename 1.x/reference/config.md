# Configuration reference

Every key of `config/app.php`. See [Configuration](../basics/configuration) for how settings and
environment variables fit together.

| Key | Type | Default in `config/app.php` | Description |
|---|---|---|---|
| `secret` | string | `APP_SECRET` (required) | Signs Datastar template URLs |
| `debug` | bool | `APP_DEBUG === '1'` | Development mode: no caches, drafts visible, exception pages |
| `url` | string | `APP_URL` (required, validated) | Public base URL, no trailing slash. Used for every absolute URL |
| `media_url` | string | `MEDIA_URL` or `''` | Where post and page files and video posters are served from: `''` (this site) or a CDN's `https://` base URL |
| `trusted_proxies` | list | from `APP_TRUSTED_PROXIES` | Reverse proxies whose `X-Forwarded-*` headers are trusted |
| `language` | string | `'en'` | The default language (no URL prefix) |
| `languages` | map | `en`, `it` | `code => ['name' => …, 'locale' => …]`; codes like `en` or `pt-br` |
| `blog.per_page` | int | `BLOG_PER_PAGE` or `20` | Posts per page on `/blog` |
| `public` | map | `[]` | Values page scripts may read through `publicConfig()`; an allowlist, refused if a value contains `APP_SECRET` |
| `csp.enabled` | bool | `true` | Send the Content Security Policy header on pages |
| `csp.report_only` | bool | `false` | Report violations in the browser console instead of blocking |
| `csp.sources` | map | `[]` | Extra sources per directive, e.g. `['frame-src' => ['https://player.vimeo.com']]` |
| `site.name` | string or map | `'Starlite'` | Site name: titles, Open Graph, feed |
| `site.description` | string or map | | Default meta description and feed subtitle |
| `site.image` | ?string or map | `null` | Default share image (`/path` or URL) |
| `site.author` | ?string or map | `null` | Author for posts and the feed; defaults to the site name |

Each `site` value can be a map `language => value` instead, see [Languages](../features/languages#the-site-s-name-description-and-share-image).

## `config/collections.php`

Data collections and their fields: `name => ['fields' => […], 'sort' => …, 'fallback' => …, 'json' => …]`.
See [Data collections](../content/collections).

## Overrides

`Kernel::boot($root, $debug, $overrides)` merges `$overrides` over the file. These extra keys are
only meant for overrides (mostly in tests):

| Key | Default | |
|---|---|---|
| `content_dir` | `<root>/content` | where `blog/` and the collection folders live |
| `cache_dir` | `<root>/var/cache` | where compiled caches are written |
| `collections` | `config/collections.php` | collection definitions, instead of the file |
