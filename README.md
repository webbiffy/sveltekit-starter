# sveltekit-starter

A personal SvelteKit template I use as the starting point for new projects. The idea is to clone or fork this once, then strip in whatever features I need without re-doing the boilerplate every time.

The stack and tooling choices reflect what I personally enjoy working with. It is not meant to be a one-size-fits-all template, just a comfortable jumping-off point.

## Stack

- SvelteKit with Svelte 5 (runes mode forced)
- TypeScript
- Vite for the dev server and build
- Tailwind CSS v4 via the official Vite plugin
- shadcn-svelte for components (Nova style, Neutral base color)
- Lucide for icons
- Inter as the default font (via @fontsource-variable/inter)
- Biome for linting and formatting JavaScript, TypeScript, JSON, and CSS
- Prettier with prettier-plugin-svelte for .svelte files only (Biome's Svelte support is still experimental)
- pnpm as the package manager

## Using this as a template

Clone or fork the repo, then:

```
pnpm install
pnpm dev
```

The dev server runs at http://localhost:5173 by default.

After cloning, the things worth changing per project are the `name` field in `package.json`, this README, the `.git` remote, and anything in `.claude/` that is project-specific.

## Scripts

- `pnpm dev` starts the Vite dev server with hot reload.
- `pnpm build` builds the production bundle.
- `pnpm preview` serves the production build locally.
- `pnpm check` runs `svelte-kit sync` and `svelte-check` against the TypeScript config.
- `pnpm lint` runs Biome on the codebase and Prettier in check mode on .svelte files.
- `pnpm format` applies Biome and Prettier fixes in place.

## Adding shadcn-svelte components

New components can be added with the shadcn-svelte CLI, for example:

```
pnpm dlx shadcn-svelte@latest add card
```

Components land in `src/lib/components/ui/` and can be imported via the `$lib/components/ui/<name>` alias.
