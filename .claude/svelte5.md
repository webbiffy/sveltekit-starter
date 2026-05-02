# Svelte 5 and SvelteKit conventions

This file is the project's reference for writing idiomatic Svelte 5 and SvelteKit code. Read it before touching any `.svelte`, `.svelte.ts`, or SvelteKit route file.

For deeper questions, consult the official LLM-friendly docs:

- Short index: https://svelte.dev/llms.txt
- Medium digest: https://svelte.dev/llms-medium.txt
- Full docs: https://svelte.dev/llms-full.txt

## Svelte 5: runes only

This project forces runes mode (see `svelte.config.js`). Use the runes API exclusively. Do not use Svelte 3/4 patterns.

State and reactivity:

- Use `$state(...)` for reactive variables. Never use a plain `let` and rely on assignment-based reactivity.
- Use `$derived(...)` for computed values. Never use `$:` reactive statements.
- Use `$derived.by(() => { ... })` when the computation needs a function body.
- Use `$effect(() => { ... })` for side effects. Never use `$:` for effects. Keep effects narrow; prefer `$derived` when the goal is a value, not a side effect.
- Use `$state.raw(...)` for large objects you do not want deeply tracked.

Props and bindings:

- Declare props with `let { foo, bar = 'default' } = $props();`. Never use `export let foo`.
- Type props with a TypeScript interface and `let { foo }: Props = $props();`.
- For two-way binding, declare with `let { value = $bindable() } = $props();`.
- Use `let { children } = $props()` and render with `{@render children()}`. Never use `<slot />`.
- For named snippets, use the `Snippet` type from `'svelte'` and render with `{@render mySnippet(arg)}`.

Events:

- Pass event handlers as props (e.g. `onclick={handleClick}`). Do not use `on:click` directive syntax.
- For DOM events on elements, use lowercase HTML attribute syntax: `<button onclick={...}>`.

Stores:

- Prefer runes over stores for new code. If you need shared cross-component state, use a `.svelte.ts` module that exports `$state` objects, not the legacy `writable`/`readable` stores.
- The `$store` auto-subscription syntax still works but is discouraged in this project.

Component shape:

```svelte
<script lang="ts">
  interface Props {
    label: string;
    count?: number;
    onincrement?: (next: number) => void;
  }

  let { label, count = 0, onincrement }: Props = $props();

  let doubled = $derived(count * 2);

  function handleClick() {
    onincrement?.(count + 1);
  }
</script>

<button {onclick} class="rounded-md border px-3 py-1">
  {label}: {count} (×2 = {doubled})
</button>
```

## SvelteKit conventions

Routing and files:

- Routes live in `src/routes/`. Files prefixed with `+` are special: `+page.svelte` (UI), `+page.ts` / `+page.server.ts` (load functions), `+layout.svelte` / `+layout.ts` / `+layout.server.ts` (layouts), `+server.ts` (API endpoints), `+error.svelte` (error UI).
- Group routes with `(group)` directories — they affect organization but not the URL.
- Dynamic segments use `[param]`; rest params use `[...rest]`; optional params use `[[param]]`.

Data loading:

- Fetch data in `load` functions, not in component lifecycle. Use `+page.server.ts` (server only, e.g. DB queries) or `+page.ts` (universal, runs on both server and client).
- Type load functions with `PageLoad` / `PageServerLoad` / `LayoutLoad` / `LayoutServerLoad` from `'./$types'`.
- Access load data in the page via `let { data }: PageProps = $props();` (using the generated `PageProps` type from `'./$types'`).
- Use `event.fetch` (not the global `fetch`) inside load functions so SvelteKit can hydrate properly.

Forms and mutations:

- Prefer SvelteKit form actions (`+page.server.ts` exporting `actions`) over manual fetch + JSON. They progressively enhance and work without JavaScript.
- Use `enhance` from `$app/forms` on `<form use:enhance>` to upgrade with client-side reactivity.

Endpoints:

- Use `+server.ts` for JSON APIs. Export `GET`, `POST`, etc. functions that return `Response` objects (or use `json()` from `'@sveltejs/kit'`).

Aliases and imports:

- Use the `$lib` alias for anything under `src/lib/` (e.g. `import { Button } from '$lib/components/ui/button';`). Do not use deep relative paths.
- Import generated types from the route-local `'./$types'` module.

Hooks:

- Server-side request handling lives in `src/hooks.server.ts`. Client-side handling in `src/hooks.client.ts`. Universal in `src/hooks.ts`.

Environment variables:

- Use `$env/static/private`, `$env/static/public`, `$env/dynamic/private`, or `$env/dynamic/public` — not `process.env` directly. Public env vars must be prefixed with `PUBLIC_`.

## When in doubt

Prefer the simpler, more idiomatic SvelteKit primitive (load function, form action, file-based route) over a hand-rolled equivalent. Check the official LLM docs linked at the top before inventing a pattern.
