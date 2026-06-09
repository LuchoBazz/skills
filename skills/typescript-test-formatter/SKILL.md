---
name: typescript-test-formatter
description: Enforces a strict data-driven test structure using describe.each and mandates modular, composable test data generation with @faker-js/faker under test/examples/.
license: MIT
compatibility: "Claude Code, Gemini, Codex, and any AI agent operating in a TypeScript/JavaScript environment."
metadata:
  author: Luis Miguel Báez (LuchoBazz)
  version: "1.0"
---

# TypeScript Data-Driven Testing & Domain Examples Protocol

You are an expert testing assistant specializing in TypeScript ecosystem practices. Your core objective is to write, format, and refactor tests to match a strict data-driven layout while ensuring all mock data is driven by highly structured, composable factories using `@faker-js/faker`.

---

## 1. Test Architecture & Structure Rules

All unit/integration tests must strictly utilize the `describe.each` pattern for data-driven testing. Do not use standalone `it` blocks for individual variations. Consolidate both happy paths and edge cases into the matrix array.

### Strict Test Layout Blueprint:
```typescript
describe.each([
  {
    description: "should [expected behavior] when [happy path condition]",
    // ... input parameters
    expected: // ... expected output or state,
  },
  {
    description: "should [expected behavior] when [edge case condition]",
    // ... input parameters
    expected: // ... expected output or state,
  },
])("given [Context/Function/Method Description]",
  ({ description, ..., expected }) => {

    beforeEach(() => {
      // Setup or reset state/mocks here if required
    });

    it(description, () => {
      const result = functionName(...);
      expect(result).toEqual(expected);
    });
  }
);
```

### Analysis Requirements:

* **Happy Paths:** Cover the standard, expected operational flows.
* **Edge Cases:** You MUST systematically analyze the code under test for boundaries, unexpected inputs, null/undefined values, extreme ranges, and error throwing. Ensure these variations are added as distinct objects within the `describe.each` matrix.

---

## 2. Test Data Generation & Faker Pattern

Mock data must never be hardcoded arbitrarily within test files. It must be abstracted into domain-specific example factories using `@faker-js/faker`.

### Strict Factory Blueprint:

Factories must be defined as functions accepting a single, optionally destructured object with defaults, returning the strictly typed domain object. This enables easy, partial overrides.

```typescript
import { faker } from "@faker-js/faker";

export const itemSummaryExample = ({
  itemId = faker.string.uuid(),
  name = `${faker.commerce.productMaterial()} ${faker.commerce.product()}`,
  slot = faker.helpers.objectValue(ItemSlotEnum), 
  powerBonus = faker.number.int({ min: 5, max: 150 }),
} = {}): ItemSummary => ({
  itemId,
  name,
  slot,
  powerBonus,
});

export const equipmentDetailsExample = ({
  totalPowerBonus = faker.number.int({ min: 100, max: 2000 }),
  items = [itemSummaryExample(), itemSummaryExample()],
} = {}): EquipmentDetails => ({
  totalPowerBonus,
  items,
});

export const characterStatsExample = ({
  level = faker.number.int({ min: 1, max: 99 }),
  experience = faker.number.int({ min: 0, max: 500000 }),
  equipment = equipmentDetailsExample(),
} = {}): CharacterStats => ({
  level,
  experience,
  equipment,
});

```

### Usage Pattern within Tests:

Use factories to instantiate baselines, overriding only properties relevant to the specific test scenario:

```typescript
// Standard setup
const maxLevelCharacter = characterStatsExample({ 
  level: 99,
  experience: 500000 
});

// Overriding deep configurations
const overpoweredCharacter = characterStatsExample({
  level: 5,
  equipment: equipmentDetailsExample({
    totalPowerBonus: 9999 // Overwriting specific nested domain logic
  })
});
```

---

## 3. File Organization & Domain Discovery

You must maintain a clean, domain-driven file separation for all testing data factories.

1. **Location:** All data factory files must reside inside the `test/examples/` directory.
2. **Domain Identification:** Before creating a new factory file, scan `test/examples/` to see if a file associated with that domain already exists (e.g., `user.example.ts`, `item.example.ts`, `character.example.ts`).
3. **Action Rule:**
* If the domain file **exists**, append your new factory functions to it.
* If the domain file **does not exist**, create a new file named following the format `[domain].example.ts` or matching the existing codebase convention, then implement the required factories.
