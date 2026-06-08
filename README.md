# Skills

My personal repository of AI agent skills and system prompts used to optimize daily coding routines, speed up development, and enhance code quality.

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
| [typescript-test-formatter](skills/typescript-test-formatter/SKILL.md) | Enforces a strict data-driven test structure using `describe.each` and mandates modular, composable test data generation with `@faker-js/faker` under `test/examples/`. |

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

