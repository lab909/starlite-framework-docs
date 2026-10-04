# Testing

```sh
npm run build          # once: the app's templates include the Vite manifest
composer test          # PHPUnit; in DDEV: ddev composer test
composer analyse       # PHPStan, level 8
```

## Two suites

| Suite | Directory | What it tests |
|---|---|---|
| **framework** | `lib/tests/` | Starlite itself, against a small fixture project and fixture content. Travels with `lib/` to every site |
| **app** | `tests/` | your site's controllers, routes and templates, with fixture content |

Run one with `vendor/bin/phpunit --testsuite app`.

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

`AppTestCase` boots your site with the fixture content in `tests/data/content`, one post per page
and a temporary cache directory. Its helpers:

| Helper | |
|---|---|
| `$this->app($debug = false, $overrides = [])` | boots the site; overrides merge over `config/app.php` |
| `$this->request($app, $uri, $method, $headers, $body)` | handles a request |
| `self::body($response)` | the body of any response, including Datastar streams and files |
| `$this->datastarUrl($html, $template)` | the Datastar URL rendering `$template`, from a page |
| `$this->tempDir()`, `$this->copyToTemp()`, `self::write()` | temporary files, deleted after the test |

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
