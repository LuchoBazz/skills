<h1 align="center">
  <code>LuchoBazz/Skills</code>
</h1>

<p align="center">
My personal repository of AI agent skills and system prompts used to optimize daily coding routines, speed up development, and enhance code quality.
</p>

---

## What is a Skill?

A **skill** is a structured instruction file (`.md`) that teaches an AI agent how to perform a specific, repeatable task with precision and consistency. Each skill defines rules, patterns, and examples the agent must follow when invoked.

Skills follow the [Agent Skills Specification](https://agentskills.io/specification).

---

## Repository Structure

```
skills/
├── skills/                        # All skill definitions
│   └── typescript-test-formatter/ # Skill: TypeScript data-driven test formatting
│       └── SKILL.md
├── spec/
│   └── agent-skills-spec.md       # Reference to the Agent Skills Specification
├── template/
│   └── SKILL.md                   # Template for creating new skills
└── README.md
```

---

## Available Skills

| Skill | Description |
|-------|-------------|
| [prompt-clarifier](skills/prompt-clarifier/SKILL.md) | Analyzes prompts, specifications, and instruction documents for ambiguities, generating a targeted Q&A template to refine requirements before execution. |
| [typescript-autonomous-security-remediation](skills/typescript-autonomous-security-remediation/SKILL.md) | Autonomous protocol for identifying, upgrading, and remediating Critical, High, and Moderate vulnerabilities using npm audit, resolving transitive overrides, and refactoring breaking changes based on official documentation. |
| [typescript-test-formatter](skills/typescript-test-formatter/SKILL.md) | Enforces a strict data-driven test structure using `describe.each` and mandates modular, composable test data generation with `@faker-js/faker` under `test/examples/`. |
| [typescript-utility-best-practices](skills/typescript-utility-best-practices/SKILL.md) | Enforces the use of `es-toolkit` for standard utility operations to prevent code duplication and optimize bundle performance. |
| [docusaurus-search-local](skills/docusaurus-search-local/SKILL.md) | Adds fully offline local full-text search to a Docusaurus v2/v3 + TypeScript project using `@easyops-cn/docusaurus-search-local`, with typed config and no external search service. |
| [docusaurus-bun-setup](skills/docusaurus-bun-setup/SKILL.md) | Sets up Bun as the package manager and runtime for a Docusaurus v2/v3 + TypeScript project — installs Bun, migrates lockfile, configures `package.json`, pins the version locally and in CI (GitHub Actions). |

---

## Adding a New Skill

1. Copy the [template/SKILL.md](template/SKILL.md) into a new folder under `skills/`.
2. Name the folder after the skill using `kebab-case`.
3. Fill in the `name`, `description` frontmatter fields and the instruction body.
4. Add the skill to the table above.

```
skills/
└── your-skill-name/
    └── SKILL.md
```

---

## License

[MIT](LICENSE)

