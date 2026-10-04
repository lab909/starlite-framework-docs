# Templates

Templates are [Twig](https://twig.symfony.com) files in `templates/`. In production they're
compiled to PHP classes in `var/cache/twig` (by `deploy` or on first use).

## The layout

`templates/_layout.twig` is the page shell. Two parts of it matter to Starlite:

```twig
<html lang="{{ site.language }}">
<head>
    {% block seo %}{% endblock %}   {# pages can set metadata here… #}
    {{ seo_tags() }}                {# …printed once: title, description, Open Graph, hreflang, JSON-LD #}
    {{ vite('resources/js/app.js') }}
</head>
```

A page sets its metadata either in its controller (`$this->app->seo->title(…)`) or in its
template, in the `seo` block that renders before the tags are printed:

```twig
{% extends '_layout.twig' %}
{% block seo %}{% do seo.title('About').description('Who we are.') %}{% endblock %}
```

## What Starlite adds to Twig

| Name | Kind | Example |
|---|---|---|
| `path(name, params, language)` | function | `{{ path('blog_post', {slug: post.slug}) }}` |
| `t` | filter | `{{ 'Load more'\|t }}`, `{{ '{n} posts'\|t({n: 3}) }}` |
| `t(message, params)` | function | `{{ t('Blog') }}` |
| `vite(entry, …)` | function | `{{ vite('resources/js/app.js') }}` |
| `seo_tags()` | function | prints the page's metadata |
| `absolute_url(path)` | function | `{{ absolute_url('/blog') }}` → `https://example.com/blog` |
| `language_switcher()` | function | the current page in every language |
| `format_date`, `format_number`… | filters | `{{ post.date\|format_date('long', locale: site.locale) }}` ([Twig Intl](https://twig.symfony.com/doc/3.x/filters/format_date.html)) |
| `site` | global | `site.name`, `site.language`, `site.locale` |
| `seo` | global | `{% do seo.title('…') %}` |
| `blog` | global | `blog.all`, `blog.tags`, `blog.page(2)` |
| `datastar` | global | `datastar.get('_partials/search')` ([Datastar](./datastar)) |

Plus the Datastar tags `patch_elements`, `patch_signals`, `remove_elements`, `execute_script` and
`location` used in partials. The [Twig reference](../reference/twig) has every signature.

## Partials

Files in `templates/_partials/` are fragments: included by pages, and rendered on their own by
Datastar requests. The leading underscore is a convention; Datastar can render any template.

## Error pages

`templates/_error.twig` renders every error (404, 405, 500) with two variables, `status` and
`message`. Error pages get `noindex` automatically.

## Escaping

Twig escapes everything by default. Use `|raw` only for HTML you trust; the blog's rendered post
HTML is safe to print raw, because Markdown is parsed with raw HTML escaped and unsafe links removed.

## Debug mode

With `APP_DEBUG=1`, templates aren't cached and `strict_variables` is on: using an undefined
variable is an error instead of an empty string. Use `??` for optional variables:
`{{ signals.q ?? '' }}`.
