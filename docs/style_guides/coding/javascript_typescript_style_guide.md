# Standalone JavaScript and TypeScript Coding Style Guide

## Document Information
- **Version**: 2.1
- **Purpose**: To provide a complete, self-contained coding standard for JS/TS projects, incorporating enterprise-wide principles.
- **Audience**: JavaScript and TypeScript Developers, Technical Leads, Code Reviewers

---

## 1. Core Principles

### 1.1 Code Quality Fundamentals
- **Readability First**: Write code for humans. Clear, self-documenting code is paramount.
- **Consistency**: Adhere to these standards, enforced by Prettier and ESLint.
- **Explicit Over Implicit**: Avoid magic strings and numbers. Use named constants.
- **Fail Fast**: Throw errors early and handle them gracefully.

### 1.2 SOLID Principles
All code must adhere to SOLID design principles:
- **S**ingle Responsibility: Each class or function should do one thing.
- **O**pen/Closed: Use composition or plugins to extend behavior without modifying source code.
- **L**iskov Substitution: Subclasses should be interchangeable with their base classes.
- **I**nterface Segregation: Define small, specific interfaces (in TypeScript).
- **D**ependency Inversion: Depend on abstractions (interfaces or tokens), not on concrete classes.

---

## 2. Naming Conventions

- **Classes, Interfaces, Enums, Types**: `PascalCase`
- **Functions and Variables**: `camelCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Files**: `kebab-case.ts` (e.g., `user-service.ts`)

---

## 3. Code and Project Structure

### 3.1 Project Structure (Feature-based)
Organize files by feature to keep related logic together.

### 3.2 Modularity (ES6 Modules)
- Use `export` and `import` for all module-related operations.
- Use `index.ts` barrel files to create a clean public API for each feature directory.

---

## 4. Documentation Standards

### 4.1 Requirement Traceability
- **File Headers**: Every source file must include a header documenting the requirements it implements.
- **API/Function Documentation**: All public APIs, classes, and methods must document the requirements they satisfy using a consistent tag.

**Tag Format**: `@requirement REQ-ID [Brief description]`

### 4.2 TSDoc
- Use TSDoc for all exported members.
- Include `@param`, `@returns`, `@throws`.

**Example:**
```typescript
/**
 * Processes a payment transaction.
 * @param customerId - The ID of the customer.
 * @returns A promise that resolves with the payment result.
 * @throws {PaymentError} If processing fails.
 * @requirement REQ-PAY-001 Process customer payments
 */
async function processPayment(customerId: string): Promise<PaymentResult> {
    // ...
}
```

### 4.3 Typing (TypeScript)
- **Avoid `any`**: Use `unknown` and perform type-checking.
- **`interface` vs `type`**: Use `interface` for object shapes that can be extended. Use `type` for unions, intersections, or primitives.
- **`readonly`**: Use `readonly` for properties that should not be mutated.

---

## 5. Asynchronous Code

- **`async/await`**: Always prefer `async/await` for handling Promises.
- **Error Handling**: Always wrap `await` calls in `try/catch` blocks.
- **Concurrency**: Use `Promise.all` or `Promise.allSettled` for running operations in parallel.

---

## 6. Testing

- **Framework**: Use Jest or Vitest.
- **Structure**: Follow the Arrange-Act-Assert pattern.
- **Descriptive Names**: Use `describe` and `it` blocks to create readable test scenarios.
- **Coverage**: Aim for >80% line coverage for new code.

---

## 7. Version Control (Git)

- **Branching**: Use the GitFlow pattern (`main`, `develop`, `feature/*`, `hotfix/*`).
- **Commit Messages**: Follow the Conventional Commits standard (`feat(auth): add password reset endpoint`).

---

## 8. Tooling

- **Linter**: ESLint with TypeScript support.
- **Formatter**: Prettier.
- **Compiler**: Enable `strict` mode in `tsconfig.json`.
