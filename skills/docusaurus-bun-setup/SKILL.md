---
name: docusaurus-bun-setup
description: Sets up Bun as the package manager and runtime for a Docusaurus v2/v3 + TypeScript project — installs Bun, migrates lockfile, configures package.json, pins the version locally and in CI (GitHub Actions), and documents what not to do.
license: MIT
compatibility: "Claude Code, Cursor, or any agentic coding assistant with file read/write and terminal access inside a Docusaurus TypeScript project."
metadata:
  author: Luis Miguel Báez (LuchoBazz)
  version: "1.0"
---

# Docusaurus + Bun Setup

## Goal
Replace npm/yarn/pnpm with Bun as the sole package manager and runtime for a Docusaurus project, or verify an existing Bun setup is correct and complete.

---

## Steps

### 1. Install Bun locally

```bash
curl -fsSL https://bun.sh/install | bash
```

Verify:

```bash
bun --version
```

If the repo already has a `.tool-versions` file pinning Bun, install through the version manager instead:

```bash
asdf install
# or
mise install
```

---

### 2. Remove conflicting lockfiles

Only `bun.lock` should exist. Delete any other lockfiles before proceeding:

```bash
rm -f package-lock.json yarn.lock pnpm-lock.yaml
```

> Do not commit `package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`. Only `bun.lock` should be tracked.

---

### 3. Install dependencies with Bun

```bash
bun install
```

This generates `bun.lock`. Commit it.

For reproducible installs (CI, fresh clones):

```bash
bun install --frozen-lockfile
```

---

### 4. Configure `package.json`

Add the following fields if not already present:

```json
{
  "engines": {
    "bun": ">=1.0.0"
  },
  "trustedDependencies": [
    "@swc/core",
    "core-js",
    "core-js-pure"
  ],
  "overrides": {
    "webpackbar": "7.0.0"
  }
}
```

- **`engines.bun`** — declares the minimum Bun version. Bun warns if the running version is older.
- **`trustedDependencies`** — Bun blocks postinstall scripts by default (security feature). `@swc/core` and `core-js` require their native build steps; list them here. Add any future dependency that needs a postinstall script to this array instead of disabling the feature globally.

> Do not add a `packageManager` field — that field is for Corepack/npm ecosystems. Bun uses `engines.bun` and `.tool-versions` instead.

---

### 5. Pin the Bun version locally

Create or update `.tool-versions` at the repo root:

```
bun 1.3.14
```

Replace `1.3.14` with the desired version. `asdf` and `mise` read this file automatically.

---

### 6. Update `.gitignore`

Ensure these entries are present:

```
/node_modules
/build
.docusaurus
bun-error.log
```

---

### 7. Run Docusaurus scripts through Bun

All existing `package.json` scripts work unchanged — just invoke them with `bun run`:

```bash
bun run start              # docusaurus start (dev server)
bun run build              # docusaurus build
bun run serve              # docusaurus serve
bun run clear              # docusaurus clear
bun run swizzle            # docusaurus swizzle
bun run deploy             # docusaurus deploy
bun run typecheck          # tsc
bun run write-translations
bun run write-heading-ids
```

Direct CLI invocation:

```bash
bun run docusaurus <command>
```

---

### 8. Configure CI (GitHub Actions)

Minimal deploy workflow pattern:

```yaml
- name: Set up Bun
  uses: oven-sh/setup-bun@v2
  with:
    bun-version: "1.3.14"   # pin explicitly; omit to read from .tool-versions

- name: Install dependencies
  run: bun install --frozen-lockfile

- name: Build Docusaurus
  run: bun run build
```

Reuse this pattern for every workflow (tests, lint, PR checks).

#### Optional: cache Bun's install directory

Add this step before `bun install` to speed up CI:

```yaml
- name: Cache Bun dependencies
  uses: actions/cache@v4
  with:
    path: ~/.bun/install/cache
    key: ${{ runner.os }}-bun-${{ hashFiles('bun.lock') }}
    restore-keys: |
      ${{ runner.os }}-bun-
```

---

### 9. Optional: `bunfig.toml`

Do not create `bunfig.toml` unless you need it. Add one only for:

- Custom or scoped registry (`[install] registry = "..."`)
- Exact version pinning (`[install] exact = true`)
- Other install behavior overrides

---

## Verify

```bash
bun run build    # must pass with no errors
bun run start    # dev server must start and serve the site
```

---

## Do Not

- Do not run `npm install`, `yarn install`, or `pnpm install` — this creates a conflicting lockfile.
- Do not delete `bun.lock`.
- Do not remove `engines.bun` or `trustedDependencies` from `package.json`.
- Do not add a `packageManager` field (Corepack/npm concept, irrelevant to Bun).
- Do not disable Bun's postinstall script-blocking globally — use `trustedDependencies` per package.

---

## Checklist

- [ ] Bun installed locally and version verified
- [ ] Conflicting lockfiles removed (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`)
- [ ] `bun install` run and `bun.lock` committed
- [ ] `engines.bun` set in `package.json`
- [ ] `trustedDependencies` includes `@swc/core` and `core-js` (and any other postinstall packages)
- [ ] `.tool-versions` pins the Bun version
- [ ] `.gitignore` covers `node_modules`, `build`, `.docusaurus`, `bun-error.log`
- [ ] CI workflow uses `oven-sh/setup-bun@v2` with pinned version and `--frozen-lockfile`
- [ ] `bun run build` and `bun run start` pass with no errors
