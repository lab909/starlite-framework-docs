# Philosophy

A few principles explain most of Starlite's design decisions. Knowing them makes the rest of the
documentation predictable.

## Compile ahead of time

Most of a website changes only when you deploy. Starlite moves the work there:
`bin/console deploy` compiles routes, templates, blog posts, translations and the asset manifest
into PHP files, and the web server's Opcache keeps them in memory. At request time, PHP mostly
looks things up.

In development (`APP_DEBUG=1`) nothing is cached, so every change shows up immediately.

## No database

Content is Markdown in `content/`, configuration is PHP in `config/` plus environment variables,
and anything per-visitor lives in the browser. No database means nothing to migrate, back up or
secure, and a deploy is a file copy.

## Proven libraries, little glue

Starlite doesn't reinvent infrastructure. Requests and responses come from
`symfony/http-foundation`, routing from `symfony/routing`, CSRF protection from
`symfony/security-csrf`, Markdown from `league/commonmark`, structured data from
`spatie/schema-org`, translations from `symfony/translation`, and assets from Vite. Starlite's own
code is the layer that ties them together: the kernel, the Datastar Twig extension, the blog
compiler, the Vite helper.

## Every page is the same for everyone

There are no sessions. CSRF protection checks the browser's origin headers instead of a token, so
pages carry no per-visitor data and can be cached publicly (every page gets an ETag and revalidates
with a cheap `304`). Absolute URLs come from configuration, never from the request's `Host` header,
so a forged header can't poison a cached page.

## Fail loudly

A misplaced post, a broken image link, a date that doesn't match its folder, a translation for a
language that isn't configured: Starlite stops with a message naming the file, in the browser
during development and in `deploy`. Silent failures are the ones that reach production.

## The framework is a dependency

A site starts from the skeleton and installs the framework (`starlite/framework`) with Composer.
It extends Starlite from the outside, through `config/bootstrap.php`, `src/`, `templates/` and
`content/`, and never edits `vendor/`. Framework improvements reach every site with
`composer update starlite/framework`.
See [Building a site on Starlite](./extending/).
