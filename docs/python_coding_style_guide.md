# Standalone Python Coding Style Guide

## Document Information
- **Version**: 2.1
- **Purpose**: To provide a complete, self-contained coding standard for Python projects, incorporating enterprise-wide principles.
- **Audience**: Python Developers, Technical Leads, Code Reviewers

---

## 1. Core Principles

### 1.1 Code Quality Fundamentals
- **Readability First**: Follow PEP 8. Code should be clear and easy to understand.
- **Consistency**: Adhere to these standards to ensure the codebase is uniform.
- **Explicit Over Implicit**: "Explicit is better than implicit" (The Zen of Python).
- **Fail Fast**: Raise exceptions as soon as an error is detected.

### 1.2 SOLID Principles
All code must adhere to SOLID design principles:
- **S**ingle Responsibility: Each class or function should have one, and only one, reason to change.
- **O**pen/Closed: Open for extension, but closed for modification.
- **L**iskov Substitution: Subtypes must be substitutable for their base types.
- **I**nterface Segregation: Prefer smaller, more specific interfaces over large, general-purpose ones.
- **D**ependency Inversion: Depend on abstractions (like abstract base classes), not on concrete implementations.

---

## 2. Naming Conventions (PEP 8)

- **Modules and Packages**: `snake_case`
- **Classes**: `PascalCase`
- **Functions, Methods, and Variables**: `snake_case`
- **Constants**: `UPPER_SNAKE_CASE`
- **Private members**: `_leading_underscore`

---

## 3. Code and Project Structure

### 3.1 Project Structure
Organize projects by feature to promote high cohesion.

### 3.2 Imports
- Use absolute imports.
- Order imports: standard library, third-party, then local application imports, each group separated by a blank line.

---

## 4. Documentation Standards

### 4.1 Requirement Traceability
- **File Headers**: Every source file must include a header documenting the requirements it implements.
- **API/Function Documentation**: All public APIs, classes, and methods must document the requirements they satisfy using a consistent tag.

**Tag Format**: `Requirement: REQ-ID [Brief description]` (within the docstring)

### 4.2 Docstrings (PEP 257)
- Use Google-style docstrings for all public modules, functions, classes, and methods.
- Include `Args:`, `Returns:`, and `Raises:` sections.

**Example:**
```python
def process_payment(customer_id: str, amount: Decimal) -> PaymentResult:
    """Processes a payment transaction.

    Args:
        customer_id: The ID of the customer.
        amount: The payment amount.

    Returns:
        The result of the payment.

    Raises:
        PaymentError: If processing fails.

    Requirement: REQ-PAY-001 Process customer payments
    """
    # ...
```

### 4.3 Type Hinting (PEP 484)
- All new code must include type hints for function signatures.

---

## 5. Error Handling

- **Custom Exceptions**: Define a custom exception hierarchy inheriting from `Exception`.
- **Context Managers**: Use the `with` statement for managing resources.

---

## 6. Testing

- **Framework**: Use `pytest`.
- **Structure**: Follow the Arrange-Act-Assert pattern.
- **Descriptive Names**: Test names should be explicit (e.g., `test_should_raise_error_on_invalid_email`).
- **Coverage**: Aim for >80% line coverage for new code.

---

## 7. Version Control (Git)

- **Branching**: Use the GitFlow pattern (`main`, `develop`, `feature/*`, `hotfix/*`).
- **Commit Messages**: Follow the Conventional Commits standard (`feat(auth): add password reset endpoint`).
