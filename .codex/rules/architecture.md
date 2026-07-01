# Architecture Patterns

## Business Logic 

- **Service Layer**: Used for external API integrations and complex/large business logic. Simple CRUD and thin controller logic does not belong in a service.
- **Repository Pattern**: not used — rely on Eloquent models directly

## Frontend

- **Inertia.js** with Vue.js — frontend built as SPA via server-driven routing
- **Domain Organization**: features organized by domain (Auth, Posts, Categories, etc.)

## Database

- Every DB structure change → new migration
- Every DB data change → update seeder + factory
- Prefer Eloquent models over raw queries (`DB::` facade)
- Prefer Eloquent relationships over manual joins
- Prefer Eloquent eager loading over lazy loading (N+1 prevention)
- Prefer Eloquent pagination, scopes, soft deletes over raw alternatives