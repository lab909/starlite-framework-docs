# Starlite documentation

Source of the Starlite docs, built with [VitePress](https://vitepress.dev) and published to GitHub
Pages by `.github/workflows/deploy.yml` on every push to `main`.

```sh
npm install
npm run dev       # http://localhost:5174 (in DDEV: https://<project>.ddev.site:5174)
npm run build     # static site in .vitepress/dist
```

Pages live in a folder per Starlite version (`1.x/`), so links keep working when 2.x arrives:
copy the folder, add it to the sidebar and the version menu in `.vitepress/config.mts`.

Starlite maintainers clone this repository into the skeleton's gitignored `docs/` folder, next to
the framework in `packages/starlite`, so code and docs change together. See the skeleton's
[CONTRIBUTING.md](https://github.com/lab909/starlite/blob/main/CONTRIBUTING.md).
