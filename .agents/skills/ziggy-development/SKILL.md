---
name: ziggy-development
description: "Use this skill for Ziggy JS which exposes Laravel named routes to the frontend via the route() helper. ALWAYS use this skill when frontend code needs to call backend routes. Trigger when: connecting Vue/Inertia frontend to Laravel routes, building forms or links to backend endpoints, fixing route-related TypeScript errors, or using route() in Vue components. Use route() instead of hardcoded URLs. Covers: route() helper, route parameters, query strings, route model binding, current route checks. Do not use for backend-only tasks."
license: MIT
metadata:
  author: tightenco
---

# Ziggy Development

This project uses **Ziggy** (`tightenco/ziggy` + `ziggy-js`) to expose Laravel named routes to the frontend via the `route()` helper.

## Setup (Already Configured)

The `ZiggyVue` plugin is registered in `app.js`, and `@routes` is injected via Blade — `route()` is available globally in all Vue templates.

TypeScript: add to a `.d.ts` file if type errors appear in templates:

```typescript
declare module 'vue' {
    interface ComponentCustomProperties {
        route: typeof routeFn;
    }
}
```

## Import in `<script setup>`

When using `route()` inside `<script setup>` (not just the template), import explicitly:

```typescript
import { route } from 'ziggy-js'
```

## Basic Usage

```typescript
// No params
route('home')                                       // "https://myapp.com/"

// Route model binding — pass an object with the binding key
route('posts.show', { post: 1 })                    // "/posts/1"

// Multiple params
route('venues.events.show', { venue: 1, event: 2 }) // "/venues/1/events/2"

// Query string — extra keys not matching route params become query string
route('posts.index', { page: 2, sort: 'asc' })      // "/posts?page=2&sort=asc"

// Mixed: route param + query string
route('posts.show', { post: 1, lang: 'en' })        // "/posts/1?lang=en"
```

## Check Current Route

```typescript
route().current()                               // 'posts.show' — current route name
route().current('posts.show')                   // true / false
route().current('posts.*')                      // wildcard match
route().current('posts.show', { post: 4 })      // match with specific param value
route().current('posts.show', { hosts: 'all' }) // also matches query params
```

## Access Current Route Params

```typescript
route().params        // { post: '4', lang: 'en' } — all params incl. query
route().routeParams   // { post: '4' }             — route params only
route().queryParams   // { lang: 'en' }             — query params only
```

## With Inertia `useForm`

```typescript
import { route } from 'ziggy-js'

const form = useForm({ title: '', body: '' })

function submit(): void {
    form.post(route('posts.store'), {
        onSuccess: () => form.reset(),
    })
}

function update(id: number): void {
    form.patch(route('posts.update', { post: id }))
}
```

## With Inertia `<Link>`

```vue
<Link :href="route('posts.index')">All Posts</Link>
<Link :href="route('posts.show', { post: post.id })">View</Link>
<Link :href="route('posts.destroy', { post: post.id })" method="delete" as="button">Delete</Link>
```

## With `router.visit`

```typescript
import { router } from '@inertiajs/vue3'
import { route } from 'ziggy-js'

router.visit(route('posts.index'))
router.visit(route('posts.update', { post: id }), {
    method: 'patch',
    data: form,
})
```

## In Templates (no import needed — global via ZiggyVue plugin)

```vue
<template>
    <a :href="route('home')">Home</a>
    <a :href="route('posts.show', { post: post.id })">{{ post.title }}</a>
    <span :class="{ active: route().current('posts.*') }">Posts</span>
</template>
```

## Common Pitfalls

- Never hardcode URL strings — always use `route()`
- Route names must match exactly what's in Laravel (`php artisan route:list`)
- Pass route model binding params as a named object: `{ post: id }` — not positional
- Extra keys in the params object automatically become query string parameters
- In `<script setup>`, import `route` from `'ziggy-js'`; in templates it's global via the plugin