# Building a site on Starlite

A new site is a clone of the Starlite repository. The site adds its own routes, controllers,
templates, content and commands, and **never edits `lib/`**.

## The rule: never edit `lib/`

Everything site-specific lives outside the framework:

| What | Where |
|---|---|
| Routes | `config/routes.php` |
| Settings | `config/app.php`, `.env` |
| Services, Twig extensions, deploy steps | `config/bootstrap.php` |
| Controllers, commands, other classes | `src/` (namespace `App\`) |
| Templates | `templates/` |
| Posts | `content/` |
| UI texts | `translations/` |
| CSS, JS, Vite entry points | `resources/`, `vite.config.js` |
| Tests | `tests/` |

Keeping `lib/` untouched means framework improvements merge cleanly into your site:

```sh
git remote add starlite git@github.com:lab909/starlite-framework.git   # once, if not done at clone time
git pull starlite main
```

If you find yourself needing to change `lib/`, that's a missing extension point: open an issue or
contribute it to Starlite instead.

## The extension points

| Extension point | Use it for | Guide |
|---|---|---|
| `config/bootstrap.php` | services, Twig extensions and globals, deploy steps | [Services & bootstrap](./bootstrap) |
| `$app->container` | shared objects (a mailer, an API client) | [Services & bootstrap](./bootstrap#services) |
| `src/Command/` | console commands, registered automatically | [Console commands](./commands) |
| `$app->addDeployStep()` | build steps in `bin/console deploy` | [Deploy steps](./deploy-steps) |
| `Starlite\Controller` | controllers with helpers | [Controllers](../basics/controllers) |
| `vite.config.js` | entry points, extra Vite plugins | [Frontend & Vite](../features/frontend) |

## A typical small app

Starlite is not opinionated about your application code. A small app usually is:

- the **blog**, for articles around the app's subject
- one or more **custom routes and controllers**: the app itself, for instance an interactive home
  page driven by Datastar, plus JSON endpoints its JavaScript loads (`$this->json(…)`)
- **services** for anything shared (`config/bootstrap.php`)
- **commands** for build or maintenance tasks, hooked into `deploy` when they produce files the
  site serves

## Housekeeping when you start a site

- Replace the Datastar demo page and the sample blog posts
- Set the real site name, description, default share image and author in `config/app.php`
- Choose the languages (`language`, `languages`) and fill `translations/*.php`
- Prepare each server's `.env` (`APP_URL`, `APP_SECRET`, `APP_OPCACHE`…), see
  [Servers & Opcache](../deployment/servers)
