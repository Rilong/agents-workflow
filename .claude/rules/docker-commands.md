# Docker Environment Commands

**All commands MUST run via Laravel Sail.**

## PHP / Artisan

```bash
./vendor/bin/sail artisan make:action Domain/ActionName
./vendor/bin/sail artisan make:request Domain/RequestName
./vendor/bin/sail artisan make:model ModelName -m
./vendor/bin/sail artisan make:migration create_table_name_table
./vendor/bin/sail artisan migrate
./vendor/bin/sail artisan migrate:rollback
./vendor/bin/sail artisan optimize:clear
./vendor/bin/sail artisan ide-helper:generate
```

## Code Quality

```bash
./vendor/bin/sail exec laravel.test ./vendor/bin/pint --dirty
./vendor/bin/sail exec laravel.test ./vendor/bin/pint
./vendor/bin/sail exec laravel.test ./vendor/bin/phpstan analyse
./vendor/bin/sail exec laravel.test ./vendor/bin/rector process --dry-run
./vendor/bin/sail exec laravel.test ./vendor/bin/rector process
```

## Testing

```bash
./vendor/bin/sail artisan test
./vendor/bin/sail artisan test --coverage
./vendor/bin/sail artisan test tests/Unit/ExampleTest.php
```

## Composer

```bash
./vendor/bin/sail composer install
./vendor/bin/sail composer require vendor/package
./vendor/bin/sail composer run dev
./vendor/bin/sail composer run pint:fix
./vendor/bin/sail composer run rector:fix
```

## Frontend

```bash
./vendor/bin/sail npm run dev
./vendor/bin/sail npm run build
./vendor/bin/sail npm install
```

> **NEVER run commands outside Sail** — all dependencies exist only in the container.