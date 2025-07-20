# Standalone Python Security Best Practices Guide

## Document Information
- **Version**: 1.0
- **Purpose**: To provide a complete, self-contained security standard for Python projects, incorporating enterprise-wide principles.
- **Audience**: Python Developers, Security Engineers, Technical Leads

---

## 1. Core Security Principles

- **Defense in Depth**: Implement multiple layers of security controls.
- **Principle of Least Privilege**: Grant only the minimum access rights necessary.
- **Fail Securely**: Ensure that system failures do not compromise security.
- **Security by Design**: Integrate security into every phase of the development lifecycle.
- **Zero Trust Architecture**: Authenticate and authorize every request, regardless of its origin.

---

## 2. Input Validation and Data Sanitization

- **Validate All Inputs**: Use a library like `Pydantic` for robust data validation and serialization.
- **Prevent XSS**: When using web frameworks like Flask or Django, use their built-in templating engines (Jinja2, Django templates) which auto-escape output by default. For other cases, use a library like `bleach` to sanitize HTML.
- **Prevent SQL Injection**: Use an ORM like SQLAlchemy or Django ORM, which use parameterized queries by default.

---

## 3. Authentication and Authorization

- **Password Security**: Hash passwords using a strong, modern algorithm like `Argon2` (via the `argon2-cffi` library) or `bcrypt`.
- **MFA**: Implement Multi-Factor Authentication for sensitive accounts.
- **Session Management**: Use the secure session management features provided by your web framework (e.g., Flask sessions, Django sessions).

---

## 4. Cryptography and Data Protection

- **Encryption in Transit**: Use TLS 1.3.
- **Encryption at Rest**: Use a well-vetted cryptography library like `cryptography` for implementing AES-256 encryption.
- **Key Management**: Store secrets in environment variables or a secure vault, not in the code.

---

## 5. Secure Communication

- **API Security**: Use a token-based authentication mechanism like JWT or OAuth2. Libraries like `PyJWT` and `Authlib` can be used.
- **Timing Attacks**: Use `hmac.compare_digest` for comparing secret values to prevent timing attacks.

---

## 6. Error Handling and Logging

- **Secure Error Handling**: In production, do not expose detailed error messages or stack traces to the user.
- **Security Logging**: Use Python's built-in `logging` module to log security events.
- **Avoid Sensitive Data in Logs**: Be careful not to log sensitive information.

---

## 7. Dependency and Supply Chain Security

- **Vulnerability Scanning**: Use a tool like `pip-audit` or `safety` to scan for known vulnerabilities in your dependencies.
- **Version Pinning**: Use a `requirements.txt` or `pyproject.toml` file to pin dependency versions.

---

## 8. Security Testing

- **SAST**: Use static analysis tools like `bandit` to find common security issues in Python code.
- **DAST**: Use dynamic analysis tools to test your running application.
- **Penetration Testing**: Conduct regular penetration testing.
