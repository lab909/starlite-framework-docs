# PHP API

The public API of the classes a site uses. Everything lives in `lib/src/` (namespace `Starlite\`).

## Kernel

`Starlite\Kernel`, available as `$this->app` in controllers, `$this->app()` in commands, and as the
argument of `config/routes.php` and `config/bootstrap.php`.

| Member | |
|---|---|
| `Kernel::boot(string $root, ?bool $debug = null, array $overrides = [])` | builds the app from `config/` |
| `run()` | handles the current HTTP request (`public/index.php`) |
| `handle(Request $request): Response` | handles any request; used by tests |
| `get(path, handler, name, requirements)`, `post(…)` | add a route |
| `route(methods, path, handler, name, requirements, csrf = true)` | add a route |
| `path(name, params = [], language = null): string` | URL path in the current language |
| `t(message, params = [], language = null): string` | translate a UI text |
| `render(template, vars = []): string` | render a Twig template |
| `stream(template, vars = []): StreamedResponse` | render a template as a Datastar response |
| `error(status, message, headers = []): Response` | an error page |
| `request(): Request` | the current request |
| `addDeployStep(name, step, description, before, after)` | add a step to `deploy` |
| `$root`, `$debug`, `$cacheDir` | project root, debug mode, cache directory |
| `$twig`, `$router`, `$blog`, `$seo`, `$site`, `$container`, `$translations`, `$datastar`, `$vite` | Starlite's services |

## Controller

`Starlite\Controller`, base class for app controllers. See [Controllers](../basics/controllers).

`render()`, `stream()`, `json()`, `notFound()`, `path()`, `t()`, `get()`, `request()`, and `$this->app`.

## Site

`Starlite\Site` (`$app->site`, `site` in Twig).

| Member | |
|---|---|
| `$name`, `$description`, `$image`, `$author`, `$baseUrl` | from `config/app.php` |
| `$defaultLanguage`, `$languages` | language configuration |
| `language(): string`, `locale(): string` | the current language and locale |
| `prefix(?language): string` | `''` for the default language, `'/it'` otherwise |
| `localize(path, ?language): string` | `'/blog'` → `'/it/blog'` |
| `url(pathOrUrl): string` | absolute URL from `APP_URL` |
| `switcher(): array` | the current page in every language |
| `setAlternates(array $alternates, array $fallbacks = [])` | declare where the current page exists |
| `alternates(): array` | `language => path` of the current page |

## Seo

`Starlite\Seo\Seo` (`$app->seo`, `seo` in Twig). Fluent setters `title()`, `description()`,
`canonical()`, `image()`, `type()`, `noindex()`, `article()`, `schema()`; plus `url()`,
`canonicalUrl()`, `imageUrl()`, `pageTitle()`, `documentTitle()`, `render()`. See [SEO](../features/seo).

## Blog

`Starlite\Blog\Blog` (`$app->blog`, `blog` in Twig). Every method works in the current language
unless one is given.

| Method | |
|---|---|
| `all(?language): list` | published posts, newest first |
| `find(slug, ?language): ?array` | one post, or `null` if it doesn't exist in that language |
| `translations(slug): list` | the languages a post exists in |
| `page(page, query = '', tag = '', ?language): array` | `{posts, page, pages, total, has_more}` |
| `search(query = '', tag = '', ?language): list` | posts matching title, summary or tags |
| `tags(?language): array` | `tag => count`, most used first |
| `$perPage` | posts per page |

A post is an array:

| Field | |
|---|---|
| `slug`, `language`, `title`, `summary` | strings |
| `date`, `updated` | `YYYY-MM-DD` (`updated` may be `null`) |
| `image` | public URL or `null` |
| `tags` | list of strings |
| `draft` | bool |
| `reading_minutes` | int |
| `html` | the rendered post (safe to print raw) |
| `source` | e.g. `2026/09/hello-starlite/index.md` |
| `assets` | publishable files in the post folder |

`Starlite\Blog\PostSeo::apply($seo, $post)` maps a post onto the page metadata.

## Container

`Starlite\Container` (`$app->container`), PSR-11: `set(id, factoryOrValue)`, `get(id)`, `has(id)`.

## Console

- `Starlite\Console\AppCommand`: base class for commands; `app()`, `root()`
- `Starlite\Console\Console::run($root)`: what `bin/console` calls
