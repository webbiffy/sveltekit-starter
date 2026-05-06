# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Conventions

- Use `pnpm` for all package manager commands. Never `npm` or `yarn`.
- Lint and format are split: Biome owns `.js`, `.ts`, `.json`, `.css`. Prettier (with `prettier-plugin-svelte`) owns `.svelte` files only. Run `pnpm format` to apply both, `pnpm lint` to check both.
- `src/lib/components/ui/**` is vendored shadcn-svelte output. Don't hand-edit these files; re-add the component via `pnpm dlx shadcn-svelte@latest add <name>` to update.
- `src/routes/layout.css` contains Tailwind v4 directives and shadcn theme tokens; it is excluded from Biome on purpose.

## Writing Svelte and SvelteKit code

The project uses Svelte 5 (runes mode is forced) and SvelteKit's file-based routing with vertical slice architecture. Before writing or modifying any `.svelte`, `.svelte.ts`, or `+page.*` / `+layout.*` / `+server.*` file:

1. Read `.claude/structure.md` for folder organization and where to place new files.
2. Read `.claude/svelte5.md` for Svelte 5 and SvelteKit conventions.
