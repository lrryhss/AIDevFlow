# Standalone C++ Coding Style Guide

## Document Information
- **Version**: 2.1
- **Purpose**: To provide a complete, self-contained coding standard for C++ projects, incorporating enterprise-wide principles.
- **Audience**: C++ Developers, Technical Leads, Code Reviewers
- **Scope**: All C++ development (C++17 and above).

---

## 1. Core Principles

### 1.1 Code Quality Fundamentals
- **Readability First**: Write clear, simple, and maintainable code.
- **Consistency**: Adhere to these standards to ensure a uniform codebase.
- **Explicit Over Implicit**: Make intentions clear. Avoid complex template metaprogramming where a simpler design would suffice.
- **Fail Fast**: Use assertions and exceptions to catch errors early.

### 1.2 SOLID Principles
All code must adhere to SOLID design principles:
- **S**ingle Responsibility: Each class should have a single, well-defined purpose.
- **O**pen/Closed: Design classes to be extensible without modification.
- **L**iskov Substitution: Derived classes must be usable wherever their base class is expected.
- **I**nterface Segregation: Prefer small, focused interfaces (abstract classes).
- **D**ependency Inversion: Depend on abstractions, not concrete implementations.

---

## 2. Naming Conventions

- **Classes, Structs, Enums**: `PascalCase`
- **Functions and Methods**: `camelCase`
- **Variables**: `snake_case`
- **Private Member Variables**: `snake_case_` (with a trailing underscore)
- **Constants and Enum Values**: `kConstantName` or `UPPER_SNAKE_CASE`
- **Namespaces**: `snake_case`
- **Files**: `snake_case.hpp`, `snake_case.cpp`

---

## 3. Code and Project Structure

### 3.1 Project Structure
Organize by feature or component.

### 3.2 Header Files (`.hpp`)
- **Header Guards**: Use `#pragma once`.
- **Include Order**: System headers, then library headers, then project headers. Use `<>` for system/library headers and `""` for project headers.

---

## 4. Documentation Standards

### 4.1 Requirement Traceability
- **File Headers**: Every source file must include a header documenting the requirements it implements.
- **API/Function Documentation**: All public APIs, classes, and methods must document the requirements they satisfy using a consistent tag.

**Tag Format**: `@requirement REQ-ID [Brief description]`

### 4.2 Doxygen
- Use Doxygen for documenting public APIs.

**Example:**
```cpp
/**
 * @brief Processes a payment transaction.
 * @param customer_id The ID of the customer.
 * @return The result of the payment.
 * @throws PaymentException if processing fails.
 * @requirement REQ-PAY-001 Process customer payments
 */
PaymentResult processPayment(const std::string& customer_id);
```

---

## 5. Modern C++ Best Practices (C++17 and above)

- **Resource Management (RAII)**: Use RAII for all resource management. `std::unique_ptr` and `std::shared_ptr` are mandatory for dynamic memory.
- **`auto`**: Use `auto` to improve readability, especially with complex types, but do not use it for public APIs where the type is part of the contract.
- **`const` and `constexpr`**: Use `const` correctness rigorously. Use `constexpr` for compile-time evaluation.
- **Smart Pointers**: Always use `std::make_unique` and `std::make_shared` instead of `new`.

---

## 6. Error Handling

- **Exceptions**: Use exceptions for reporting and handling errors. Derive custom exceptions from `std::exception`.
- **`std::optional`**: Use for functions that may legitimately not return a value.
- **`std::variant` / `std::expected`**: Use for returning one of several types or a value/error pair.

---

## 7. Testing

- **Framework**: Use Google Test (`gtest`).
- **Structure**: Follow the Arrange-Act-Assert pattern.
- **Descriptive Names**: Test names should be explicit (e.g., `TEST_F(MyClassTest, ShouldThrowExceptionOnNullInput)`).
- **Coverage**: Aim for >80% line coverage for new code.

---

## 8. Version Control (Git)

- **Branching**: Use the GitFlow pattern (`main`, `develop`, `feature/*`, `hotfix/*`).
- **Commit Messages**: Follow the Conventional Commits standard (`feat(auth): add password reset endpoint`).

---

## 9. Tooling

- **Static Analysis**: `clang-tidy` and `cppcheck`.
- **Formatter**: `clang-format`.
- **Build System**: CMake.