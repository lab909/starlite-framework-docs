---
layout: home

hero:
  name: Starlite
  text: Dynamic sites at static speed
  tagline: A tiny, database-free PHP micro framework. Twig, Datastar, a Markdown blog, translations and SEO, compiled ahead of time for Opcache.
  image:
    src: /logo.svg
    alt: Starlite
  actions:
    - theme: brand
      text: Get started
      link: /1.x/getting-started/installation
    - theme: alt
      text: What is Starlite?
      link: /1.x/

features:
  - title: No database, no admin panel
    details: Content lives in Markdown files, configuration in PHP and .env. Everything a request needs is compiled into var/cache and served from Opcache's shared memory.
  - title: Reactive without a JS framework
    details: Datastar drives the UI over server-sent events; Twig partials render on the server. A Craft-style Twig API makes it a one-liner.
  - title: Content, languages, SEO
    details: A blog with post folders, images, drafts and translated posts; /it/-style language URLs; Open Graph, JSON-LD, hreflang, sitemap and Atom feeds.
  - title: Proven parts, little glue
    details: Symfony components, Twig, CommonMark and Vite do the heavy lifting. Starlite is the small layer that ties them together, and stays readable.
  - title: Built to be cloned
    details: A new site is a clone that never edits lib/. Commands, services, Twig extensions and deploy steps plug in from the app.
  - title: Tested and analysed
    details: 130+ tests across the framework and the app, PHPStan level 8, and a CI workflow from day one.
---
