# Console commands

```sh
php bin/console list        # in DDEV: ddev console list
```

## `deploy`

Optimizes the autoloader, rebuilds every cache, publishes post files and refreshes the web
server's Opcache. See [Deploying](../deployment/) for the options and
[Deploy steps](../extending/deploy-steps) for the steps.

```sh
php bin/console deploy [--list-steps] [--skip=a,b] [--assets] [--opcache=cachetool|reload|none]
                       [--fcgi=…] [--cachetool=…] [--reload-cmd=…] [--composer=…] [--no-dev]
                       [--skip-composer] [--skip-opcache]
```

Exit codes: `0` deployed, `1` a step failed, `2` invalid options (nothing was changed).

## `cache:clear`

Deletes everything in `var/cache` (keeping `.gitkeep`) and the post files published to
`public/media/blog/`. If a deploy left the Composer autoloader authoritative, it also runs
`composer dump-autoload`, so new classes are found again. Run it when you go back to development
after a deploy.

| Option | Default | |
|---|---|---|
| `--composer` | `composer` | Composer binary |

## Your commands

Every command class in `src/Command/` is registered automatically. See
[Console commands](../extending/commands).
