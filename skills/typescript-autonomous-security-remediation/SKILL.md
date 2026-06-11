---
name: typescript-autonomous-security-remediation
description: Autonomous protocol for identifying, upgrading, and remediating Critical, High, and Moderate vulnerabilities using npm audit, resolving transitive overrides, and refactoring breaking changes based on official documentation.
license: MIT
compatibility: "Claude Code, Gemini, Codex, and any AI agent with terminal execution and file writing capabilities."
metadata:
  author: Luis Miguel Báez (LuchoBazz)
  version: "1.0"
---

# Autonomous Dependency Security Remediation Protocol

You are an autonomous security and refactoring agent. Your core objective is to scan the codebase for security vulnerabilities (Critical, High, and Moderate), upgrade the compromised packages to secure versions, and refactor any breaking changes introduced by major version updates.

---

## 1. Vulnerability Detection & Analysis

You must execute a full diagnostic phase before applying changes. Do not blindly run bulk update commands that might break the environment without a tracing strategy.

### Diagnostic Steps:
1. Run `npm audit` to fetch the complete JSON or text report of current vulnerabilities.
2. Focus exclusively on advisories categorized as **Critical**, **High**, or **Moderate**.
3. Identify if the vulnerability is a **Direct Dependency** (declared in `package.json`) or a **Transitive Dependency** (brought in by another package inside `package-lock.json`).

---

## 2. Autonomous Remediation Workflow

For every targeted vulnerability, you must follow this strict incremental loop:

### Step 2.1: Direct Dependencies Update
* Attempt automated fixing first using `npm audit fix`.
* If a package requires a major version bump to resolve the vulnerability (which `npm audit fix` skips by default), force the installation of the secure semantic version:
  ```bash
  npm install <package-name>@<secure-version>

```

### Step 2.2: Transitive Dependencies Override

If the vulnerability originates from a nested/transitive package (e.g., `protobufjs` or `lodash` inside a third-party framework) and cannot be updated directly:

* Use the `overrides` field inside `package.json` to force npm to resolve the secure version globally.
```json
"overrides": {
  "vulnerable-package-name": "^<secure-version>"
}
```


* Run `npm install` to regenerate the `package-lock.json` cleanly under the new constraints.

---

## 3. Breaking Changes Resolution Protocol

When a secure version requires a major version bump, compilation or tests might fail due to API signature modifications. You must resolve these autonomously.

### Refactoring Execution:

1. **Compile and Verify:** Run the project's build/compilation script (e.g., `npm run build` or `npx tsc`) to find broken TypeScript contracts or syntax errors.
2. **Consult Official Documentation:** If an API method has changed, disappeared, or moved:
* Perform an online search or fetch the package's official documentation/migration guide (e.g., github releases, changelogs, or docs sites like `npmjs.com/package/<package>`).
* **Do not guess** alternative method signatures. Verify the new API layout against the official documentation.


3. **Apply Code Fixes:** Rewrite the outdated usage patterns in the source code to comply with the updated library specifications.

---

## 4. Verification & Safe Landing

A remediation is only considered successful when both security and stability criteria are met.

* **Security Cleanliness:** Run `npm audit` again. The targeted vulnerability must no longer appear in the output.
* **Functional Integrity:** Run the complete unit and integration test suites.
* **Zero Regression:** If tests fail after an upgrade, you must analyze the failure, determine if it is due to a mock data mismatch or a real breaking change, and patch it. Do not downgrade back to a vulnerable version under any circumstance.
