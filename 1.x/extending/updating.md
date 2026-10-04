# Updating Starlite

The framework is the `starlite/framework` Composer package, so updating it never touches your
site's files:

```sh
ddev composer update starlite/framework
```

Then run your checks and look at the result before deploying:

```sh
ddev composer test
ddev composer analyse
```

## Versions

Starlite is in beta. Until the first tagged release, the skeleton requires the development branch:

```json
"require": { "starlite/framework": "^1.0@dev" }
```

`dev-main` is aliased as `1.x-dev`, so `^1.0@dev` follows the `main` branch of
[lab909/starlite-framework](https://github.com/lab909/starlite-framework). Once releases are
tagged (`v1.0.0`…), switch to `^1.0` to receive only tagged versions within 1.x. A future 2.x would
need a deliberate change of the constraint, with its own [documentation version](/).

Your `composer.lock` records the exact framework commit, so every server installs the version you
tested. Commit it after each update.

## Where the package comes from

Starlite isn't on Packagist. The skeleton's `composer.json` points Composer at GitHub:

```json
"repositories": [
    { "type": "path", "url": "packages/*", "options": { "symlink": true } },
    { "type": "vcs", "url": "https://github.com/lab909/starlite-framework" }
]
```

The first entry is for Starlite maintainers, who work on the framework in a local clone in
`packages/` (see [Contributing](../contributing)). In your site, `packages/` stays empty and
Composer uses GitHub. You can delete the first entry and the `packages/` folder if you never work on
the framework itself.

## Skeleton changes

Improvements to the skeleton (new example templates, config defaults) don't reach existing sites
automatically, because your site's files are yours. When a framework release needs a change in
your files, its notes and these docs say so.
