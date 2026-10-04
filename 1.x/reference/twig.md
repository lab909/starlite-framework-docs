# Twig reference

## Globals

| Global | Type | |
|---|---|---|
| `site` | `Starlite\Site` | `site.name`, `site.description`, `site.baseUrl`, `site.language`, `site.locale`, `site.defaultLanguage`, `site.languages` |
| `seo` | `Starlite\Seo\Seo` | page metadata setters: `{% do seo.title('…') %}` (see [SEO](../features/seo)) |
| `blog` | `Starlite\Blog\Blog` | `blog.all`, `blog.find(slug)`, `blog.page(n, q, tag)`, `blog.search(q, tag)`, `blog.tags`, `blog.perPage` |
| `datastar` | `Starlite\Datastar` | Datastar actions, below |

Plus anything your `config/bootstrap.php` adds.

## Functions

| Function | Returns |
|---|---|
| `path(name, params = {}, language = null)` | URL path of a named route, in the current (or given) language |
| `absolute_url(pathOrUrl)` | absolute URL from `APP_URL`; absolute `http(s)` URLs are returned unchanged |
| `t(message, params = {}, language = null)` | translated text |
| `vite(entry, …)` | `<link>`/`<script>` tags for Vite entry points (dev server or build); entries or lists of entries |
| `vite_preload(source, …)` | `<link rel="preload">` for built fonts and images, by source path (e.g. a Fontsource `.woff2`); nothing with the dev server |
| `theme_script()` | inline `<script>` setting `<html data-theme>` from the saved choice or the system setting; first in `<head>` |
| `public_config()` | `<script type="application/json" id="starlite-config">` with config `public`, or nothing when empty |
| `seo_tags()` | the page's `<title>`, meta, canonical, Open Graph, Twitter, hreflang and JSON-LD |
| `language_switcher()` | list of `{code, name, url, active, available}` for the current page |
| `patch_signals(signals, options = {})` | Datastar: queue a signals patch (use with `do`) |
| `remove_elements(selector, options = {})` | Datastar: queue an element removal |
| `execute_script(script, options = {})` | Datastar: queue a script |
| `location(uri, options = {})` | Datastar: queue a redirect |

## Filters

| Filter | |
|---|---|
| `t(params = {}, language = null)` | `{{ 'Hello {name}'\|t({name: user}) }}` |
| `patch_elements(options = {})` | Datastar: queue the content as an elements patch (`{% apply patch_elements %}…{% endapply %}`) |
| `format_date`, `format_datetime`, `format_time`, `format_number`, `format_currency`, … | from [Twig Intl Extra](https://twig.symfony.com/doc/3.x/filters/format_date.html); pass `locale: site.locale` |

## `datastar`

| Method | Prints |
|---|---|
| `datastar.get(template, vars = {}, options = {})` | `@get(…)` rendering `template` with `vars` (also `post`, `put`, `patch`, `delete`) |
| `datastar.action(method, url, options = {})` | `@method(url, options)` for any URL |

`options` are Datastar's own action options, e.g. `{openWhenHidden: true}`.

Templates rendered by Datastar requests receive the browser's signals as `signals`.
