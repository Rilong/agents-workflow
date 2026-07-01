# PHP & Eloquent Code Style

## Strict PHP

- All PHP files must declare `declare(strict_types=1)`
- Full type hints required for all parameters and return types
- Use `===` instead of `==` (strict comparisons)
- Use PHP 8.4 features and modern type casting
- Trailing commas in multiline arrays and parameters

## Class Organization

Specific order for class elements:
1. Constants
2. Properties
3. Methods

## Eloquent Conventions

- Use `query()` method for model queries (not static `Model::where(...)`)
- Eager loading: `with()`, `withCount()`, `withTrashed()`
- Prefer Eloquent relationships, scopes, pagination, soft deletes over raw queries

## Code Quality Tools

| Tool | Purpose | Config |
|------|---------|--------|
| Laravel Pint | Code formatting | Laravel preset with strict rules |
| Cognitive Complexity | Complexity limits | class: 85, function: 8 |
