# Project structure

```
my-site/
├── bin/console              Console entry point: deploy, cache:clear, your commands
├── config/
│   ├── app.php              Settings: secret, URL, languages, blog, site defaults
│   ├── bootstrap.php        The app's extension point: services, Twig additions, deploy steps
│   └── routes.php           Routes → controllers
├── content/blog/            Blog posts, one folder each (see "The blog")
├── packages/                Empty (.gitkeep); used by Starlite maintainers, see Contributing
├── public/                  Web root: index.php, build/ (Vite), media/ (published post files)
├── resources/               Frontend sources: js/app.js, js/pages/*.js, css/app.css
├── src/                     Your PHP code (namespace App\): Controller/, Command/, …
├── templates/               Twig templates; _partials/ are rendered by Datastar requests
├── tests/                   Your app's tests
├── translations/            UI texts per language: en.php, it.php, …
├── var/cache/               Compiled caches (safe to delete)
├── vendor/starlite/framework/  ★ The framework, installed by Composer: never edit it
├── .env                     Local environment (never committed)
├── composer.json            Your app's dependencies; requires starlite/framework
└── vite.config.js           Your entry points and Vite plugins
```

## The framework is a package

The framework is the `starlite/framework` package from
[lab909/starlite-framework](https://github.com/lab909/starlite-framework). Composer installs it
into `vendor/starlite/framework`, together with its own dependencies (Symfony components, Twig,
CommonMark…); your `composer.json` only requires `starlite/framework`. Framework code never
depends on `App\` classes.

::: warning Never edit vendor/
Everything a site needs to change has an extension point in your own files. Framework updates
arrive with `composer update starlite/framework` (see [Updating Starlite](../extending/updating)),
which would overwrite any edit in `vendor/`.
:::

## What's yours, what's Starlite's

| Yours (edit freely) | Starlite's (don't edit in a site) |
|---|---|
| `config/`, `src/`, `templates/`, `content/`, `translations/`, `resources/`, `tests/` | `vendor/starlite/framework/` |
| `public/index.php`, `bin/console` (three-line entry points) | its Vite plugin, `resources/vite/starlite.js` |
| `vite.config.js` (entry points, extra plugins) | its test base class, `Starlite\Testing\KernelTestCase` |

## Namespaces

| Namespace | Directory |
|---|---|
| `Starlite\` | `vendor/starlite/framework/src/` |
| `App\` | `src/` |
| `App\Tests\` | `tests/` |
