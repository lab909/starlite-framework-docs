# Contributing

Starlite is three repositories:

| Repository | What it is |
|---|---|
| [lab909/starlite](https://github.com/lab909/starlite) | the site skeleton: the starting point for every site |
| [lab909/starlite-framework](https://github.com/lab909/starlite-framework) | the framework package, `starlite/framework` |
| [lab909/starlite-framework-docs](https://github.com/lab909/starlite-framework-docs) | this documentation (VitePress) |

## Working on all three together

Changes often span the framework, the skeleton and the docs. Clone the other two repositories
**inside** the skeleton; both locations are gitignored, so they're never published with it:

```sh
git clone git@github.com:lab909/starlite.git && cd starlite
git clone git@github.com:lab909/starlite-framework.git packages/starlite
git clone git@github.com:lab909/starlite-framework-docs.git docs
ddev start
ddev composer update starlite/framework     # symlinks packages/starlite into vendor/
```

The skeleton's `composer.json` lists a path repository (`packages/*`) before the framework's
GitHub repository. When `packages/starlite` exists, Composer prefers it and symlinks it into
`vendor/starlite/framework`, so framework edits take effect immediately. Because of that, the
skeleton never commits `composer.lock`.

Each clone is its own git repository: commit framework changes in `packages/starlite`, docs
changes in `docs/`, skeleton changes at the root.

## Running the checks

```sh
ddev composer test && ddev composer analyse                                  # skeleton (app tests)
cd packages/starlite && composer update && composer test && composer analyse # framework
cd docs && npm install && npm run build                                      # docs
```

The docs dev server (`npm run dev` in `docs/`) runs on port 5174; DDEV exposes it at
`https://<project>.ddev.site:5174`.

## Guidelines

- Keep the framework free of `App\` code: what a site might change belongs in the skeleton, or
  needs an extension point in the framework.
- Every behaviour change comes with a test (framework or app suite) and a docs update.
- Prefer proven libraries for infrastructure; Starlite's own code is the glue between them.
- PHPStan level 8 stays clean in both PHP repositories.

## Docs fixes

Every page has an **Edit this page on GitHub** link: small fixes can go straight to the docs
repository as a pull request.
