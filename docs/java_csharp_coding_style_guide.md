# Standalone Java and C# Coding Style Guide

## Document Information
- **Version**: 2.1
- **Purpose**: To provide a complete, self-contained coding standard for Java and C# projects, incorporating enterprise-wide principles.
- **Audience**: Java and C# Developers, Technical Leads, Code Reviewers

---

## 1. Core Principles

### 1.1 Code Quality Fundamentals
- **Readability First**: Code is read far more often than it is written. Optimize for human understanding.
- **Consistency**: Adhere to these standards to ensure the codebase is uniform and predictable.
- **Explicit Over Implicit**: Make code intentions clear through explicit naming and typing.
- **Fail Fast**: Detect and report errors as early as possible with clear, actionable error messages.

### 1.2 SOLID Principles
All code must adhere to SOLID design principles:
- **S**ingle Responsibility: Each class should have one reason to change.
- **O**pen/Closed: Open for extension, closed for modification.
- **L**iskov Substitution: Derived classes must be substitutable for their base classes.
- **I**nterface Segregation: Clients should not depend on interfaces they don't use.
- **D**ependency Inversion: Depend on abstractions, not on concrete implementations.

---

## 2. Naming Conventions

### 2.1 Universal Rules
- **Descriptive and Meaningful**: Names must clearly indicate purpose (e.g., `customerBirthDate`, `validateUserCredentials()`).
- **Avoid Ambiguous Abbreviations**: Use complete words (e.g., `userManager`, not `usrMgr`).

### 2.2 Java/C# Specifics
- **Classes, Enums, Interfaces, Records**: `PascalCase`
- **Methods and Variables**: `camelCase`
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `public static final int MAX_RETRIES = 3;`)
- **Packages (Java)**: `lowercase.with.dots` (e.g., `com.company.project.module`)
- **Namespaces (C#)**: `PascalCase.With.Dots` (e.g., `Company.Project.Module`)
- **Interfaces (C#)**: Use the `IPascalCase` convention (e.g., `IPaymentProcessor`).

---

## 3. Code and Project Structure

### 3.1 Logical Grouping
Organize files by feature or domain, not by technical layer, to promote high cohesion.

### 3.2 Class Structure
1.  Public static fields
2.  Private static fields
3.  Instance fields (public, then private)
4.  Constructors
5.  Public methods
6.  Private helper methods

### 3.3 Method Design
- **Single Responsibility**: A method should do one thing well.
- **Parameter Count**: Aim for 3 or fewer parameters. Use a parameter object (DTO) for more complex inputs.
- **Line Length**: Keep lines under 120 characters.

---

## 4. Documentation Standards

### 4.1 Requirement Traceability
- **File Headers**: Every source file must include a header documenting the requirements it implements.
- **API/Function Documentation**: All public APIs, classes, and methods must document the requirements they satisfy using a consistent tag.

**Tag Format**: `@requirement REQ-ID [Brief description]`

### 4.2 API Documentation
- **Java**: Use Javadoc for all public classes, interfaces, and methods.
- **C#**: Use XML documentation comments (`///`).

**Example (Java):**
```java
/**
 * Processes a payment transaction.
 * @param customerId The ID of the customer.
 * @return The result of the payment.
 * @requirement REQ-PAY-001 Process customer payments
 */
public PaymentResult processPayment(String customerId) { /*...*/ }
```

**Example (C#):**
```csharp
/// <summary>
/// Processes a payment transaction.
/// </summary>
/// <param name="customerId">The ID of the customer.</param>
/// <returns>The result of the payment.</returns>
/// <requirement>REQ-PAY-001 Process customer payments</requirement>
public PaymentResult ProcessPayment(string customerId) { /*...*/ }
```

### 4.3 Implementation Comments
- Comment on the *why*, not the *what*.
- Use `// TODO:` for planned work and `// FIXME:` for known issues that need fixing.

---

## 5. Error Handling and Logging

### 5.1 Exception Handling
- **Specific Exceptions**: Create and use a custom exception hierarchy for your application.
- **Resource Management**: Always use `try-with-resources` (Java) or `using` statements (C#).

### 5.2 Logging
- **Use a standard framework**: SLF4j with Logback (Java), Serilog (C#).
- **Use structured logging**: Log key-value pairs (JSON).
- **Never log sensitive data**: Mask passwords, PII, and API keys.

---

## 6. Testing

- **Arrange-Act-Assert (AAA)**: Structure tests clearly.
- **Descriptive Names**: Test names should describe the scenario and expected outcome (e.g., `shouldThrowExceptionWhenEmailIsInvalid`).
- **Coverage**: Aim for >80% line coverage and >70% branch coverage for new code.

---

## 7. Version Control (Git)

- **Branching**: Use the GitFlow pattern (`main`, `develop`, `feature/*`, `hotfix/*`).
- **Commit Messages**: Follow the Conventional Commits standard (`feat(auth): add password reset endpoint`).
