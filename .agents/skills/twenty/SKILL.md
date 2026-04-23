```markdown
# twenty Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill introduces the core development patterns, coding conventions, and automated workflows used in the `twenty` monorepo. The repository is primarily TypeScript-based, using React on the frontend, and includes backend, shared, and infrastructure packages. It emphasizes automation for translation, GraphQL schema/type generation, database migrations, and CI/CD, with a focus on maintainability and consistency.

## Coding Conventions

**File Naming**
- Use `camelCase` for file names.
  - Example: `userProfile.ts`, `orderService.ts`

**Import Style**
- Mixed: Both default and named imports are used.
  - Example:
    ```typescript
    import React from 'react';
    import { useState } from 'react';
    import userService from './userService';
    ```

**Export Style**
- Mixed: Both default and named exports are present.
  - Example:
    ```typescript
    // Named export
    export function calculateTotal() { ... }

    // Default export
    export default UserProfile;
    ```

**Commit Patterns**
- Prefixes: `fix`, `chore`, `feat` (freeform after prefix)
- Example commit messages:
  - `fix: correct user role assignment`
  - `feat: add password reset endpoint`
  - `chore: update dependencies`

## Workflows

### i18n Translation Sync
**Trigger:** When translations are updated or new locales are added  
**Command:** `/sync-i18n`

1. Update `.po` files for each locale in frontend and/or backend:
    - `packages/twenty-front/src/locales/*.po`
    - `packages/twenty-server/src/engine/core-modules/i18n/locales/*.po`
2. Regenerate corresponding `.ts` translation files in `generated` directories:
    - `packages/twenty-front/src/locales/generated/*.ts`
    - `packages/twenty-server/src/engine/core-modules/i18n/locales/generated/*.ts`
3. Commit all updated and generated translation files.

**Example:**
```bash
# After editing .po files
/sync-i18n
git add .
git commit -m "chore: sync translations for new locale"
```

---

### GraphQL Schema and Types Regeneration
**Trigger:** When backend GraphQL schema changes  
**Command:** `/regenerate-graphql-types`

1. Update backend GraphQL schema.
2. Regenerate:
    - `packages/twenty-client-sdk/src/metadata/generated/schema.graphql`
    - `packages/twenty-client-sdk/src/metadata/generated/schema.ts`
    - `packages/twenty-client-sdk/src/metadata/generated/types.ts`
    - `packages/twenty-front/src/generated-metadata/graphql.ts`
3. Update or add related frontend/backend files as needed.

**Example:**
```bash
# After updating backend schema
/regenerate-graphql-types
git add packages/twenty-client-sdk/src/metadata/generated/
git add packages/twenty-front/src/generated-metadata/graphql.ts
git commit -m "feat: regenerate GraphQL types for new API fields"
```

---

### Add or Update Database Migration
**Trigger:** When a new database table, column, or constraint is added or modified  
**Command:** `/new-migration`

1. Create or update migration file(s) in:
    - `packages/twenty-server/src/database/typeorm/core/migrations/**/*.ts`
2. Update migration utility files if needed:
    - `packages/twenty-server/src/database/typeorm/core/migrations/utils/**/*.ts`
3. Update or add related command files for running/managing migrations:
    - `packages/twenty-server/src/database/commands/upgrade-version-command/**/*.ts`
    - `packages/twenty-server/src/database/commands/run-typeorm-migration.command.ts`
4. Update schema or entity files as needed.

**Example:**
```bash
# After editing migration files
/new-migration
git add packages/twenty-server/src/database/typeorm/core/migrations/
git commit -m "feat: add migration for user preferences table"
```

---

### Feature Development with Tests and Docs
**Trigger:** When developing a new feature or fixing a bug  
**Command:** `/new-feature`

1. Edit or add implementation files (service, resolver, or UI component):
    - `packages/twenty-server/src/engine/**/*.ts`
    - `packages/twenty-front/src/modules/**/*.ts*`
    - `packages/twenty-shared/src/**/*.ts`
2. Edit or add related test files:
    - `packages/twenty-server/test/**/*.ts`
    - `packages/twenty-front/src/modules/**/*.test.ts*`
3. Edit or add related constants or utility files.
4. Update documentation or type definitions if needed.

**Example:**
```typescript
// Implementation: packages/twenty-front/src/modules/user/userProfile.tsx
export function UserProfile() { ... }

// Test: packages/twenty-front/src/modules/user/userProfile.test.ts
import { describe, it, expect } from 'vitest';
import { UserProfile } from './userProfile';

describe('UserProfile', () => {
  it('renders user name', () => {
    // test implementation
  });
});
```

---

### CI Workflow Update
**Trigger:** When CI/CD pipeline needs to be fixed, improved, or updated  
**Command:** `/update-ci`

1. Edit one or more `.github/workflows/*.yaml` files.
2. Update related scripts or Dockerfiles if needed:
    - `packages/twenty-docker/**/*.sh`
    - `packages/twenty-docker/**/*.Dockerfile`
3. Commit changes to CI configuration.

**Example:**
```bash
# After editing workflow YAML
/update-ci
git add .github/workflows/build.yaml
git commit -m "chore: update CI to use Node 18"
```

## Testing Patterns

- **Framework:** [vitest](https://vitest.dev/)
- **Test File Pattern:** Files end with `.test.ts`
  - Example: `userService.test.ts`
- **Test Structure:**
    ```typescript
    import { describe, it, expect } from 'vitest';
    import { calculateTotal } from './orderService';

    describe('calculateTotal', () => {
      it('returns correct sum', () => {
        expect(calculateTotal([1, 2, 3])).toBe(6);
      });
    });
    ```

## Commands

| Command                    | Purpose                                                      |
|----------------------------|--------------------------------------------------------------|
| /sync-i18n                 | Synchronize translation files and regenerate translation TS   |
| /regenerate-graphql-types  | Regenerate GraphQL schema and TypeScript types               |
| /new-migration             | Create or update database migration files                    |
| /new-feature               | Start a new feature or bugfix with tests and docs            |
| /update-ci                 | Update CI/CD workflow files and related scripts              |
```
