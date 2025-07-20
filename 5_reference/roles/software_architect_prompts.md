# AI Prompts for Software Architects

This document provides AI prompts tailored for Software Architects to assist with system design, technology selection, architectural governance, and ensuring alignment with enterprise standards, adhering to the project's SOPs and best practices.

## 1. Solution Architecture Design

### 1.1 Designing High-Level Architecture

**Prompt**: "Given the `[Requirements Document ID]` (`[path/to/requirements.xml]`), propose a high-level solution architecture for `[System/Module Name]`. Include major components, their interactions, data flow, and potential technology choices. Reference the [Design Principles](../../1_principles/1.3_design_principles.md) and [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md)."

### 1.2 Selecting Technology Stack

**Prompt**: "We need to select a database for `[Component/Service Name]`. Compare `[Database A, e.g., PostgreSQL]` and `[Database B, e.g., MongoDB]` based on our non-functional requirements (e.g., scalability, consistency, performance for `[specific use case]`). Recommend the best fit with justification, considering our [Design Principles](../../1_principles/1.3_design_principles.md)."

### 1.3 Defining Non-Functional Requirements (NFRs) Impact

**Prompt**: "Analyze the impact of the following non-functional requirements on the proposed architecture for `[System Name]`: 
- `[NFR 1, e.g., 99.9% uptime]`
- `[NFR 2, e.g., 10,000 concurrent users]`
- `[NFR 3, e.g., Data encryption at rest and in transit]`
Suggest architectural patterns and design considerations to meet these requirements, referencing the [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md) and [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md)."

## 2. Architectural Governance

### 2.1 Reviewing Detailed Designs

**Prompt**: "Review the `[Detailed Design Document ID]` (`[path/to/ddd.md]`) for `[Component/Service Name]`. Assess its alignment with the approved Solution Architecture, adherence to [Design Principles](../../1_principles/1.3_design_principles.md), and overall technical soundness. Identify any potential architectural risks or anti-patterns."

### 2.2 Ensuring Compliance

**Prompt**: "How can we ensure that the `[System Name]` design complies with `[Compliance Standard, e.g., GDPR, PCI DSS]`? Outline the key architectural considerations and design patterns required, referencing relevant sections of our [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md) and [Requirements Writing Style Guide](../../docs/requirements_writing_style_guide.md)."

## 3. Technical Leadership & Mentorship

### 3.1 Guiding Technical Leads

**Prompt**: "A Technical Lead is struggling with `[technical challenge, e.g., designing a resilient microservice, optimizing database performance]`. Provide guidance, suggest architectural patterns, and recommend resources to help them overcome this challenge."

### 3.2 Documenting Architectural Decisions

**Prompt**: "We've decided to use `[Technology/Pattern]` for `[specific reason]`. Draft an Architecture Decision Record (ADR) for this decision, including the context, decision, consequences, and alternatives considered. Reference the [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md)."

## 4. Risk Management

### 4.1 Identifying Technical Risks

**Prompt**: "For the `[Project Name]` project, what are the key technical risks associated with `[specific technology/approach, e.g., adopting a new framework, integrating with a legacy system]`? Suggest potential mitigation strategies and their impact on the architecture, referencing the [Risk Management SOP](../../docs/SOPs/risk_management_sop.md)."
