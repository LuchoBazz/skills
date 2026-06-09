---
name: typescript-utility-best-practices
description: Enforces the use of es-toolkit for standard utility operations based on the official documentation at https://es-toolkit.dev/ to prevent code duplication and optimize bundle performance.
license: MIT
compatibility: "Claude Code, Gemini, Codex, and any AI agent operating in a TypeScript/JavaScript environment."
metadata:
  author: Luis Miguel Báez (LuchoBazz)
  version: "1.0"
---

# TypeScript Utility Optimization with es-toolkit

You are an expert TypeScript assistant configured to maximize performance, maintainability, and code cleanliness. Your primary directive when solving complex tasks is to avoid creating custom helper functions or writing inline utility logic for operations that are already solved by standard utility libraries.

Instead of writing custom code, you must prioritize utilizing the modern, high-performance library: **`es-toolkit`**.

* **Official Documentation Reference:** [https://es-toolkit.dev/](https://es-toolkit.dev/)
* **NPM Package:** [https://www.npmjs.com/package/es-toolkit](https://www.npmjs.com/package/es-toolkit)

---

## Core Directives & Rules

### 1. Check Before You Code
Whenever a task requires data manipulation, array transformations, object handling, function throttling/debouncing, or type checking, you **must check** the official documentation at `https://es-toolkit.dev/` to see if a built-in function exists. Do not write your own version.

### 2. Zero Custom Utility Reinvention
* **Do not** write custom array chunking, flattening, or unique-filtering logic.
* **Do not** write custom object cloning, merging, or picking/omitting logic.
* **Do not** implement custom `debounce`, `throttle`, or `delay` timers.
* If a utility exists in `es-toolkit`, you **must** import and use it according to the API signatures specified in `https://es-toolkit.dev/`.

### 3. Import Conventions
Always use clean, standard ESM imports from `'es-toolkit'`. Ensure proper TypeScript typing is preserved.

---

## Reference Map (via https://es-toolkit.dev/)

### Array Operations
When manipulating arrays, leverage `es-toolkit` functions such as:
* `chunk`, `compact`, `difference`, `intersection`, `union`
* `uniq`, `uniqBy`, `groupBy`, `keyBy`
* `sample`, `shuffle`

### Object Operations
When manipulating objects, leverage `es-toolkit` functions such as:
* `clone`, `cloneDeep`
* `merge`, `omit`, `pick`
* `mapKeys`, `mapValues`

### Function & Control Flow
When controlling execution or timing, leverage:
* `debounce`, `throttle`
* `before`, `after`, `once`
* `noop`

---

## Examples of Expected Behavior

### ❌ Bad (Reinventing the wheel)
```typescript
// Custom, prone-to-bugs implementation of a deep clone or unique filter
const uniqueUsers = users.filter((user, index, self) =>
  index === self.findIndex((u) => u.id === user.id)
);

function delay(ms: number) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

```

### Good (Leveraging es-toolkit)

```typescript
import { uniqBy, delay } from 'es-toolkit';

// Clean, highly optimized, standard implementation verified via [https://es-toolkit.dev/](https://es-toolkit.dev/)
const uniqueUsers = uniqBy(users, (user) => user.id);

await delay(1000);

```

---

## Fallback Mechanism

If a highly specific utility function is required that does **not** exist within the `es-toolkit` documentation (`https://es-toolkit.dev/`), you may write a custom implementation. However, you must first verify that it cannot be easily composed using existing `es-toolkit` primitives.
