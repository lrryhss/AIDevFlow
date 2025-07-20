# AI Prompts for Security Architects

This document provides AI prompts tailored for Security Architects to assist with designing secure systems, implementing security controls, conducting threat modeling, and ensuring compliance with security policies and regulations, adhering to the project's SOPs and best practices.

## 1. Secure System Design

### 1.1 Threat Modeling

**Prompt**: "Conduct a STRIDE threat model for the `[Feature/Module Name]` of `[Application Name]`. Identify potential threats related to Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege. For each threat, suggest high-level mitigation strategies, referencing the [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md)."

### 1.2 Designing Security Controls

**Prompt**: "For the `[Authentication/Authorization/Data Protection]` component of `[System Name]`, propose a detailed design for security controls. Include considerations for `[specific aspect, e.g., password hashing, token validation, access control lists]`, referencing relevant sections of our [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md) and [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md)."

### 1.3 Secure API Design

**Prompt**: "Review the proposed API design for `[API Name]` (`[path/to/api_spec.yaml]`). Identify potential security vulnerabilities (e.g., injection flaws, broken authentication, excessive data exposure) and suggest secure design patterns and best practices to mitigate them, referencing the [OWASP API Security Top 10] and our [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md)."

## 2. Security Implementation & Review

### 2.1 Reviewing Code for Security Vulnerabilities

**Prompt**: "Review the following `[language]` code snippet for potential security vulnerabilities (e.g., SQL injection, XSS, insecure deserialization). Point out any issues and suggest secure coding practices to remediate them, referencing the [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md) and relevant language-specific security guides (e.g., [Python Security Best Practices](../../docs/python_security_best_practices.md)).
```[language]
[code snippet]
```"

### 2.2 Implementing Security Features

**Prompt**: "Outline the steps to implement `[Security Feature, e.g., Two-Factor Authentication, Role-Based Access Control]` in our `[Language/Framework]` application. Include considerations for database schema changes, API modifications, and user experience, referencing the [Implementation Principles](../../1_principles/1.4_implementation_principles.md) and [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md)."

## 3. Compliance & Risk Management

### 3.1 Ensuring Regulatory Compliance

**Prompt**: "How can we ensure that `[System Name]` complies with `[Regulatory Standard, e.g., GDPR, HIPAA]`? Outline the key technical and process requirements, and suggest a compliance checklist, referencing the [Requirements Writing Style Guide](../../docs/requirements_writing_style_guide.md) and [Risk Management SOP](../../docs/SOPs/risk_management_sop.md)."

### 3.2 Assessing Security Risks

**Prompt**: "For the `[Project Name]` project, what are the key security risks associated with `[specific aspect, e.g., third-party integrations, data storage, user authentication]`? Assess their likelihood and impact, and propose mitigation strategies, referencing the [Risk Management SOP](../../docs/SOPs/risk_management_sop.md)."

## 4. Incident Response

### 4.1 Developing Incident Response Plans

**Prompt**: "Draft an incident response plan for a `[Type of Incident, e.g., data breach, denial of service attack]` affecting `[Application Name]`. Include steps for detection, containment, eradication, recovery, and post-incident analysis."
