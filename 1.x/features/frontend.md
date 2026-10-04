# Frontend & Vite

Assets are built by [Vite](https://vite.dev) with [Tailwind CSS](https://tailwindcss.com).
Sources live in `resources/`:

```
resources/
  css/app.css              Tailwind, plus the typography plugin for blog posts
  js/app.js                imports the CSS and the Datastar client
  js/vendor/datastar.js    Datastar v1, vendored from the official bundle
```

## Development

```sh
ddev npm run dev
```

The dev server runs on port 5173 (exposed by DDEV at `https://<project>.ddev.site:5173`). While it
runs, pages load assets from it, and:

| You change | What happens |
|---|---|
| CSS | styles update in place, no reload |
| JavaScript | the page reloads |
| a Twig template, a Markdown post, a translation file | the page reloads |

This only happens with `APP_DEBUG=1`. Vite writes its URL to `var/vite.hot` while it runs, and PHP
reads it in debug mode only. If Vite was killed and pages still point at port 5173, delete
`var/vite.hot`.

## Production build

```sh
npm run build
```

This writes hashed files and source maps to `public/build/`, plus a manifest that
`{{ vite('resources/js/app.js') }}` reads to print the right `<link>` and `<script>` tags.
`bin/console deploy --assets` runs the build for you.

## `vite.config.js`

Your config only lists plugins and entry points. Starlite's part (build output, manifest, the DDEV
dev server, page reloads, the `VITE_PUBLIC_` env prefix) is a framework plugin:

```js
import { defineConfig } from 'vite';
import tailwindcss from '@tailwindcss/vite';
import starlite from './vendor/starlite/framework/resources/vite/starlite.js';

export default defineConfig({
    plugins: [
        tailwindcss(),
        starlite({ input: ['resources/js/app.js'] }),
    ],
});
```

Options: `input` (entry points) and `reload` (extra file patterns that reload the page). Anything
you set in `vite.config.js` overrides the plugin's defaults.

## Several entry points

To keep heavy JavaScript off pages that don't need it, give a page its own entry:

```js
starlite({ input: ['resources/js/app.js', 'resources/js/pages/mixer.js'] })
```

```twig
{{ vite('resources/js/app.js', 'resources/js/pages/mixer.js') }}
```

## Environment variables in JavaScript

Only variables prefixed `VITE_PUBLIC_` are ever inlined into the bundle
(`import.meta.env.VITE_PUBLIC_…`). Everything else, including `APP_SECRET`, stays on the server.
