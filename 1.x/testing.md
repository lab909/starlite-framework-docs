# Testing

```sh
npm run build          # optional: without it, the tests that check assets are skipped
composer test          # PHPUnit; in DDEV: ddev composer test
composer analyse       # PHPStan, level 8
```

## Two suites

| Suite | Where | What it tests |
|---|---|---|
| **app** | your site's `tests/` | your site's controllers, routes and templates, with fixture content |
| **framework** | the framework repository's `tests/` | Starlite itself, against a fixture project and fixture content (see [Contributing](./contributing)) |

Your site runs its own suite; the framework's suite runs in the framework repository's CI.

## No web server needed

Tests drive the app through `Kernel::handle()`, which takes a Symfony `Request` and returns a
`Response`:

```php
<?php

declare(strict_types=1);

namespace App\Tests;

final class AboutPageTest extends AppTestCase
{
    public function testAboutPageInItalian(): void
    {
        $response = $this->request($this->app(), '/it/about');

        self::assertSame(200, $response->getStatusCode());
        self::assertStringContainsString('<h1 class="text-3xl font-bold">Chi siamo</h1>', self::body($response));
    }
}
```

`AppTestCase` (in your `tests/`) extends the framework's `Starlite\Testing\KernelTestCase` and
boots your site with the fixture content in `tests/data/content`, one post per page and a
temporary cache directory. The helpers:

| Helper | |
|---|---|
| `$this->app($debug = false, $overrides = [])` | boots the site; overrides merge over `config/app.php` (`AppTestCase`) |
| `$this->bootKernel($root, $debug, $overrides)` | boots any Starlite app with a temporary cache directory (`KernelTestCase`) |
| `$this->request($app, $uri, $method, $headers, $body)` | handles a request |
| `self::body($response)` | the body of any response, including Datastar streams and files |
| `$this->datastarUrl($html, $template)` | the Datastar URL rendering `$template`, from a page |
| `$this->tempDir()`, `$this->copyToTemp()`, `self::write()` | temporary files, deleted after the test |

## Tests and the Vite build

The templates print Vite's asset tags, which come from `public/build/` after `npm run build`. Before
the first build, `KernelTestCase` makes pages render without those tags instead of failing, so every
test runs on a fresh clone. A test that checks assets declares it, and is skipped until there's a
build:

```php
public function testThePageScriptLoads(): void
{
    $app = $this->app();
    $this->requireViteBuild($app); // skipped, with "run npm run build", when there's no build

    self::assertStringContainsString('/build/assets/home-', self::body($this->request($app, '/')));
}
```

CI runs `npm run build` before the tests, so these run there.

## Requests with headers

```php
// a Datastar POST from the same site
$this->request($app, '/clock', 'POST', ['Sec-Fetch-Site' => 'same-origin'], '{}');

// revalidation
$this->request($app, '/about', 'GET', ['If-None-Match' => $etag]);
```

## Environment

`phpunit.xml.dist` sets `APP_SECRET`, `APP_URL=https://example.test`, `APP_DEBUG=0` and
`BLOG_PER_PAGE`, as real environment variables, so your `.env` never leaks into the tests.

## Continuous integration

`.github/workflows/ci.yml` runs on every push and pull request: install, `npm run build`, PHPUnit,
PHPStan.
