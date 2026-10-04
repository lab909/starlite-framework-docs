# Installation

## Requirements

- **PHP 8.4** with the `intl`, `mbstring` and `opcache` extensions
- **Composer 2**
- **Node.js 22+** and npm, for the asset build
- For local development: **[DDEV](https://ddev.com)** (recommended), which provides all of the above
  in Docker

## Create a site

A Starlite site starts from the **skeleton**, [lab909/starlite](https://github.com/lab909/starlite).
On GitHub, click **Use this template** to create your site's own repository (without the
skeleton's history), then clone it:

```sh
git clone git@github.com:you/my-site.git
cd my-site
```

::: details Without GitHub's template button
```sh
git clone --depth 1 https://github.com/lab909/starlite.git my-site
cd my-site && rm -rf .git && git init
```
:::

The framework isn't part of the skeleton: it's the `starlite/framework` package, which Composer
installs from [lab909/starlite-framework](https://github.com/lab909/starlite-framework) in the
next step. It isn't on Packagist; the skeleton's `composer.json` already points Composer at GitHub.

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
ddev composer update                 # installs starlite/framework from GitHub
ddev npm install
ddev npm run dev                     # Vite + Tailwind with hot reload
```

Open `https://my-site.ddev.site`. Editing a template, a post or a translation reloads the page;
CSS changes apply without a reload.

::: tip Commit your lock file
The skeleton doesn't commit `composer.lock`. Your site should: remove the `/composer.lock` line
from `.gitignore` and commit the file, so every server installs exactly the versions you tested.
:::

::: details Without DDEV
Any PHP 8.4 setup works. Point the web server's document root at `public/` and send every request
that isn't a file to `public/index.php`. For a quick look:

```sh
composer update && npm install && npm run build
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
