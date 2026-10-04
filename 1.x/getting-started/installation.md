# Installation

## Requirements

- **PHP 8.4** with the `intl`, `mbstring` and `opcache` extensions
- **Composer 2**
- **Node.js 22+** and npm, for the asset build
- For local development: **[DDEV](https://ddev.com)** (recommended), which provides all of the above
  in Docker

## Create a site

A Starlite site starts as a clone of the framework repository. Keep the framework as a second git
remote, so you can pull its updates later:

```sh
git clone --recurse-submodules git@github.com:lab909/starlite-framework.git my-site
cd my-site
git remote rename origin starlite
git remote add origin git@github.com:you/my-site.git   # your site's own repository
```

::: tip The docs submodule
`--recurse-submodules` also fetches this documentation into `docs/`. A site doesn't need it; you
can remove it with `git rm docs` in your clone.
:::

## Configure

Copy the example environment file and fill it in:

```sh
cp .env.example .env
```

```ini
APP_SECRET=…                                  # openssl rand -hex 32
APP_URL=https://my-site.ddev.site             # your site's public URL, no trailing slash
APP_DEBUG=1                                   # development mode
```

`APP_SECRET` signs Datastar requests; `APP_URL` is used for every absolute URL (canonical links,
Open Graph, sitemap, feeds). Both are required: the app refuses to boot without them.
See [Configuration](../basics/configuration) for everything else.

## Run it with DDEV

```sh
ddev config --project-name=my-site   # once, if you renamed the project
ddev start
ddev composer install
ddev npm install
ddev npm run dev                     # Vite + Tailwind with hot reload
```

Open `https://my-site.ddev.site`. Editing a template, a post or a translation reloads the page;
CSS changes apply without a reload.

::: details Without DDEV
Any PHP 8.4 setup works. Point the web server's document root at `public/` and send every request
that isn't a file to `public/index.php`. For a quick look:

```sh
composer install && npm install && npm run build
php -S localhost:8000 -t public public/index.php
```

The Vite dev server is configured for DDEV; outside it, run `npm run build` after asset changes, or
adjust `server.origin` in `vite.config.js`.
:::

## Check that everything works

```sh
ddev composer test       # the framework and app test suites
ddev composer analyse    # PHPStan
```

## Next steps

- [Project structure](./structure): where everything lives
- [Your first page](./first-page): a route, a controller and a template in five minutes
