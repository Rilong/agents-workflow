---
name: phpunit-testing
description: "Use this skill for PHPUnit testing in Laravel projects. Trigger whenever any test is being written, edited, fixed, or refactored — including fixing tests that broke after a code change, adding assertions, TDD workflows. Always activate when the user asks how to write something in PHPUnit, mentions test files or directories (tests/Feature, tests/Unit), or needs HTTP endpoint tests, unit tests for services/actions, or observer tests. Covers: class-based test syntax, $this->assert*(), data providers, mocking, RefreshDatabase, actingAs(), assertInertia(). Do not use for factories, seeders, migrations, controllers, models, or non-test PHP code."
license: MIT
metadata:
  author: laravel
---

# PHPUnit Testing in Laravel

## Test Organization

- Feature tests: `tests/Feature/` — HTTP endpoints, full workflows
- Unit tests: `tests/Unit/` — Services, Actions, Observers, isolated logic
- Do NOT remove tests without approval
- Architectural testing is enforced via `tests/Unit/ArchTest.php` if present

## What NOT to Test — Eloquent Models

**DO NOT** write unit tests for Laravel Eloquent models. Laravel's ORM is already extensively tested by the framework team, and models are excluded from coverage (see `phpunit.xml`). Testing standard ORM behavior provides no value.

Do **not** test:

- Basic relationships (`hasOne`, `hasMany`, `belongsTo`, etc.)
- Simple CRUD operations and standard Eloquent functionality
- Factory creation without custom logic
- Basic fillable/guarded attributes and standard casting

**Exceptions** — do test these (in Unit/Feature tests, not "model tests"):

- Custom business-logic methods
- Complex accessors/mutators with business rules
- Custom scopes with specific logic
- Observer behavior and side effects

Test model behavior *indirectly* instead: feature tests via HTTP endpoints, integration tests for model interactions, observer tests for event handlers, and Action/Service tests for business logic.

## Creating Tests

```bash
./vendor/bin/sail artisan make:test Feature/PostTest
./vendor/bin/sail artisan make:test Unit/CartServiceTest --unit
```

## Basic Test Structure

```php
<?php

declare(strict_types=1);

namespace Tests\Feature;

use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class PostTest extends TestCase
{
    use RefreshDatabase;

    public function test_authenticated_user_can_view_posts(): void
    {
        $user = User::factory()->create();

        $response = $this->actingAs($user)->get(route('posts.index'));

        $response->assertOk();
    }
}
```

## Running Tests

```bash
./vendor/bin/sail artisan test
./vendor/bin/sail artisan test --coverage
./vendor/bin/sail artisan test tests/Feature/PostTest.php
./vendor/bin/sail artisan test --filter=test_authenticated_user_can_view_posts
```

## Assertions

Use named assertion methods over `assertStatus()`:

| Use | Instead of |
|-----|-----------|
| `assertOk()` | `assertStatus(200)` |
| `assertCreated()` | `assertStatus(201)` |
| `assertNotFound()` | `assertStatus(404)` |
| `assertForbidden()` | `assertStatus(403)` |
| `assertRedirect()` | `assertStatus(302)` |
| `assertUnauthorized()` | `assertStatus(401)` |

## Inertia Assertions

```php
$response->assertInertia(
    fn ($page) => $page
        ->component('Posts/Index')
        ->has('posts')
        ->has('posts.0.title')
        ->where('posts.0.title', 'Hello World')
);
```

## Mocking

```php
use Illuminate\Support\Facades\Mail;
use Illuminate\Support\Facades\Event;
use Illuminate\Support\Facades\Queue;

Mail::fake();
Event::fake();
Queue::fake();

// After action
Mail::assertSent(OrderConfirmation::class);
Event::assertDispatched(OrderPlaced::class);
Queue::assertPushed(ProcessOrder::class);
```

## Data Providers

```php
public static function invalidEmailProvider(): array
{
    return [
        'empty string' => [''],
        'missing @' => ['notanemail'],
        'missing domain' => ['user@'],
    ];
}

#[\PHPUnit\Framework\Attributes\DataProvider('invalidEmailProvider')]
public function test_rejects_invalid_email(string $email): void
{
    $response = $this->postJson(route('auth.register'), ['email' => $email]);

    $response->assertUnprocessable();
}
```

## setUp / tearDown

```php
protected function setUp(): void
{
    parent::setUp();
    $this->user = User::factory()->create();
    $this->service = app(CartService::class);
}
```

## Common Pitfalls

- Using `assertStatus(200)` instead of named assertions
- Forgetting `parent::setUp()` when overriding `setUp()`
- Not using `RefreshDatabase` when tests touch the database
- Deleting tests without approval