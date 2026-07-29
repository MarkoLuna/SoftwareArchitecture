# TypeScript Architecture

TypeScript adds a type system to JavaScript, enabling better tooling, safer refactoring, and self-documenting code. This article covers TypeScript-specific architectural patterns.

---

## Type System Patterns

### Discriminated Unions

A union of object types distinguished by a literal property (the discriminant). Fundamental for modeling state machines and API responses.

```ts
type ApiState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: string };

// Exhaustive check — TypeScript errors if a case is missing
const renderState = (state: ApiState<User>) => {
  switch (state.status) {
    case 'idle': return null;
    case 'loading': return <Spinner />;
    case 'success': return <Profile user={state.data} />;
    case 'error': return <Error message={state.error} />;
  }
};
```

### Branded Types

A technique for creating nominal (opaque) types in TypeScript's structural type system. Prevents mixing up values of the same shape but different meanings.

```ts
type UserId = string & { readonly __brand: 'UserId' };
type PostId = string & { readonly __brand: 'PostId' };

const createUserId = (id: string) => id as UserId;
const createPostId = (id: string) => id as PostId;

const getUser = (id: UserId) => { /* ... */ };
const postId = createPostId('abc');
// getUser(postId); // Type error — PostId is not assignable to UserId
```

### Generic Constraints

Use `extends` to constrain generic type parameters while preserving flexibility.

```ts
interface HasId { id: string; }

const getById = <T extends HasId>(items: T[], id: string): T | undefined =>
  items.find(item => item.id === id);
```

---

## Module Architecture

### Barrel Files

An `index.ts` file that re-exports from multiple modules, providing a single public API for a directory.

```ts
// components/index.ts
export { Button } from './Button';
export { Input } from './Input';
export { Modal } from './Modal';

// Consumer imports
import { Button, Input, Modal } from './components';
```

### Path Aliases

Configure `paths` in `tsconfig.json` to avoid deep relative imports:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

### Declaration Files (`.d.ts`)

Ambient type declarations for JavaScript libraries without built-in types, or for augmenting existing types.

```ts
// globals.d.ts
declare global {
  interface Window {
    __APP_CONFIG__: { apiUrl: string; env: string };
  }
}
```

---

## Project Configuration

### `tsconfig.json` Strategies

| Strategy | `strict` | `strictNullChecks` | `noUncheckedIndexedAccess` | Use Case |
|---|---|---|---|---|
| Strict | `true` | `true` | optional | New projects |
| Relaxed | `false` | `true` | `false` | Gradual migration |
| Ultra-strict | `true` | `true` | `true` | Safety-critical |

### `compilerOptions` Key Settings

- `strict`: Enables all strict type-checking options (recommended for all new projects)
- `exactOptionalPropertyTypes`: Prevents `undefined` from being assigned to optional properties
- `noUnusedLocals` / `noUnusedParameters`: Catch dead code at compile time
- `isolatedModules`: Ensures each file can be transpiled independently (required by Vite, esbuild)

---

## Utility Types

TypeScript provides built-in utility types for common transformations:

| Utility | Description | Example |
|---|---|---|
| `Partial<T>` | All properties optional | `Partial<User>` → `{ name?: string; age?: number }` |
| `Required<T>` | All properties required | `Required<Partial<User>>` |
| `Pick<T, K>` | Select specific properties | `Pick<User, 'id' | 'name'>` |
| `Omit<T, K>` | Exclude specific properties | `Omit<User, 'password'>` |
| `Record<K, V>` | Object type with keys K and values V | `Record<string, User>` |
| `ReturnType<T>` | Extract return type of function | `ReturnType<typeof fetchUser>` |

---

[⬅️ Back to Home](../README.md)
