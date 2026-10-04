# Translating posts

A translation sits next to the original in the post folder, as `index.<code>.md`, and shares the
folder's images:

```
content/blog/2026/09/hello-starlite/
  index.md          → /blog/hello-starlite        (default language)
  index.it.md       → /it/blog/hello-starlite
  cover.png
```

`index.md` is always the post in the **default language** (see [Languages](../features/languages)).
The slug is the same in every language.

## What a translation contains

A translation needs its own `title`, and usually a `summary`. Everything it omits is inherited from
`index.md`:

| Field | When omitted |
|---|---|
| `title` | required |
| `summary` | the first paragraph with text, of the translation |
| `date`, `updated`, `image`, `tags` | inherited from `index.md` |

```markdown
---
title: Ciao, Starlite
summary: Un micro framework senza database.
---

Starlite è un piccolo framework PHP…
```

## Untranslated posts

A post that isn't written in a language **doesn't exist** in that language: it isn't listed,
searchable, counted in tags or put in that language's feed, and its URL is a 404. The 404 page's
language switcher still links to the versions that do exist.

A post can also exist only in a non-default language: just `index.it.md`, with its own `date`.

## Language-aware SEO

- the language switcher on a post links to its translations; a language without one links to that
  language's blog instead
- `hreflang` links list only the versions that exist
- each language has its own feed (`/it/blog/feed.xml`), and the sitemap lists every version

## Changing the default language

Because `index.md` means "the default language", switching `language` in `config/app.php` means
renaming the files: the old `index.it.md` becomes `index.md`, and the old `index.md` becomes, say,
`index.en.md`. A mismatch is reported as an error. So is a file for a language that isn't in
`languages`.
