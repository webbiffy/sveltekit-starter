# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Conventions

- Use `pnpm` for all package manager commands. Never `npm` or `yarn`.
- Lint and format are split: Biome owns `.js`, `.ts`, `.json`, `.css`. Prettier (with `prettier-plugin-svelte`) owns `.svelte` files only. Run `pnpm format` to apply both, `pnpm lint` to check both.
- `src/lib/components/ui/**` is vendored shadcn-svelte output. Don't hand-edit these files; re-add the component via `pnpm dlx shadcn-svelte@latest add <name>` to update.
- `src/routes/layout.css` contains Tailwind v4 directives and shadcn theme tokens; it is excluded from Biome on purpose.

## Writing Svelte and SvelteKit code

The project uses Svelte 5 (runes mode is forced) and SvelteKit's file-based routing. Before writing or modifying any `.svelte`, `.svelte.ts`, or `+page.*` / `+layout.*` / `+server.*` file, read `.claude/svelte5.md` for the conventions this project follows.
