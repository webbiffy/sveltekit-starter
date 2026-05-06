# Project folder structure

This project uses **vertical slicing** to organize code by feature/domain rather than by type. Each feature owns its UI, state, types, validation, and API logic.

**See also:** [`.claude/svelte5.md`](svelte5.md) for Svelte 5 syntax and SvelteKit conventions.

## Folder layout

```
src/
  features/
    [feature-name]/
      components/       # Feature-specific UI components
      stores/          # Feature state (*.svelte.ts)
      types/           # Feature type definitions
      utils/           # Feature-specific helpers, validators
      routes/          # Feature routes (mirror to src/routes/)
  routes/
    (group)/           # Route groups organize by feature
      [route]/
        +page.svelte
        +page.server.ts
    +layout.svelte
    +error.svelte
  lib/
    components/
      common/          # Design system (Button, Input, Dialog, etc.)
    utils/             # Cross-feature shared helpers
    types/             # Shared types used by multiple features
    server/            # Shared server code (DB, session, auth)
```

## How it works

Routes in `src/routes/` are thin wrappers that import from feature folders:

```svelte
<!-- src/routes/(auth)/login/+page.svelte -->
<script lang="ts">
  import LoginForm from '$lib/features/auth/components/LoginForm.svelte';
</script>

<LoginForm />
```

The feature folder contains everything needed:
- `$lib/features/auth/components/LoginForm.svelte` — the form UI
- `$lib/features/auth/stores/auth.svelte.ts` — auth state
- `$lib/features/auth/types/auth.ts` — User, Session types
- `$lib/features/auth/utils/validators.ts` — form validation logic

## Feature barrels and exports

Each feature exposes a public API via an `index.ts` barrel file. This creates a single import path and clearly defines what the feature exports:

```ts
// src/lib/features/auth/index.ts
export { default as LoginForm } from './components/LoginForm.svelte';
export { default as RegisterForm } from './components/RegisterForm.svelte';
export { auth } from './stores/auth.svelte';
export type { User, Session } from './types/auth';
export { validateEmail, validatePassword } from './utils/validators';
```

Then other features or routes import cleanly:
```ts
import { LoginForm, auth, type User } from '$lib/features/auth';
```

**Feature utilities vs. shared utilities:**
- **Feature utilities** (exported from `features/[name]/index.ts`): Domain-specific helpers used by that feature and maybe 1-2 others (e.g., `validateEmail` in auth).
- **Shared utilities** (`src/lib/utils.ts`): Generic, cross-cutting code used by many features (e.g., `cn()` for classnames, type helpers).

Keep utilities close to where they're used. Extract to `src/lib/utils.ts` only when genuinely needed by 3+ features or when they're infrastructure (not domain-specific).

## Guidelines

- **Put it in a feature folder** if it belongs to one feature only (component, store, type, util).
- **Put it in `lib/common`** if it's a reusable design system component (Button, Input, Modal).
- **Put it at the root of `src/lib/`** (e.g., `src/lib/utils.ts`, `src/lib/constants.ts`) for shared utility files used across multiple features.
- **Put it in `lib/utils`** or `lib/types`** subdirectories** if grouping related utilities or types by category (e.g., `lib/utils/date.ts`, `lib/utils/validation.ts`).
- **Put it in `lib/server`** if it's shared server-only code (database, session, auth logic).
- Route files stay in `src/routes/` and import from features; keep them as thin as possible.

## Scaling

As the project grows:
- Add new features as new folders under `features/`
- Create new route groups `(feature-name)` in `src/routes/` for organization
- Keep utilities in feature folders initially. Extract them to `lib/` only when 2+ features actually need them — don't extract prematurely.
- Keep related code together within a feature folder

