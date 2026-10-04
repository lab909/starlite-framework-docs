# Project structure

```
my-site/
├── bin/console              Console entry point: deploy, cache:clear, your commands
├── config/
│   ├── app.php              Settings: secret, URL, languages, blog, site defaults
│   ├── bootstrap.php        The app's extension point: services, Twig additions, deploy steps
│   └── routes.php           Routes → controllers
├── content/blog/            Blog posts, one folder each (see "The blog")
├── docs/                    This documentation (git submodule, optional)
├── lib/                     ★ The framework: the starlite/framework Composer package
│   ├── src/                 Kernel, Router, Datastar, Blog, Seo, Site, console commands…
│   ├── resources/vite/      Starlite's Vite plugin
│   └── tests/               The framework's own test suite
├── public/                  Web root: index.php, build/ (Vite), media/ (published post files)
├── resources/               Frontend sources: js/app.js, css/app.css, vendored Datastar
├── src/                     Your PHP code (namespace App\): Controller/, Command/, …
├── templates/               Twig templates; _partials/ are rendered by Datastar requests
├── tests/                   Your app's tests
├── translations/            UI texts per language: en.php, it.php, …
├── var/cache/               Compiled caches (safe to delete)
├── .env                     Local environment (never committed)
├── composer.json            Your app's dependencies; requires starlite/framework
└── vite.config.js           Your entry points and Vite plugins
```

## The framework is a package

`lib/` is installed through a Composer [path repository](https://getcomposer.org/doc/05-repositories.md#path)
and symlinked to `vendor/starlite/framework`, so edits take effect immediately. It declares its own
dependencies in `lib/composer.json`, and the app's `composer.json` only requires
`starlite/framework`. Framework code never depends on `App\` classes.

::: warning Never edit lib/ in a site
Everything a site needs to change has an extension point outside `lib/`. Keeping `lib/`
untouched is what lets you pull framework updates later. See
[Building a site on Starlite](../extending/).
:::

## What's yours, what's Starlite's

| Yours (edit freely) | Starlite's (don't edit in a site) |
|---|---|
| `config/`, `src/`, `templates/`, `content/`, `translations/`, `resources/`, `tests/` | `lib/` |
| `public/index.php`, `bin/console` (three-line entry points) | `lib/resources/vite/starlite.js` |
| `vite.config.js` (entry points, extra plugins) | |

## Namespaces

| Namespace | Directory |
|---|---|
| `Starlite\` | `lib/src/` |
| `App\` | `src/` |
| `Starlite\Tests\` | `lib/tests/` |
| `App\Tests\` | `tests/` |
