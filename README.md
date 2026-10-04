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

This repository is a git submodule of the framework repository (`docs/`). Write docs in the same
pull request series as the code they describe, and update the submodule pointer in the framework.
