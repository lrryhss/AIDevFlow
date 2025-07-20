# AI Prompts for Technical Leads

This document provides AI prompts tailored for Technical Leads to assist with architectural oversight, code quality, team guidance, and project management, adhering to the project's SOPs and best practices.

## 1. Architectural & Design Oversight

### 1.1 Reviewing Design Documents

**Prompt**: "Review the attached `[Design Document Name]` (`[path/to/design_doc.md]`) against the [Design Principles](../../1_principles/1.3_design_principles.md) and the [Phase 2: Architecture and Design SOP](../../docs/SOPs/phase_2_architecture_design_sop.md). Identify any deviations, potential risks, or areas for improvement in terms of scalability, security, and maintainability."

### 1.2 Technology Stack Decisions

**Prompt**: "We are considering `[Technology A]` vs `[Technology B]` for `[specific component/problem]`. Based on our [Design Principles](../../1_principles/1.3_design_principles.md), [Coding Style Guides](../../docs/style_guides/coding/), and [Security Best Practices Guides](../../docs/style_guides/security/), analyze the pros and cons of each, and recommend the most suitable option with justification."

### 1.3 Defining Implementation Strategy

**Prompt**: "Given the `[Requirements Document ID]` and `[Design Document ID]`, propose a high-level implementation strategy for the `[Module/Feature Name]`. Break it down into logical phases/components and suggest which [Implementation Guides](../../2_templates/2.4_implementation_guide.xml) would be needed, referencing the [Implementation Principles](../../1_principles/1.4_implementation_principles.md)."

## 2. Code Quality & Standards

### 2.1 Conducting Code Reviews

**Prompt**: "I'm reviewing a Pull Request for `[feature/bugfix]` with the following changes: 
```diff
[git diff output]
```
Based on our [Coding Style Guides](../../docs/style_guides/coding/), [Security Best Practices Guides](../../docs/style_guides/security/), and the [Git Workflow and Best Practices SOP](../git_sop.md), identify potential issues, suggest improvements, and provide constructive feedback."

### 2.2 Enforcing Coding Standards

**Prompt**: "We need to enforce `[specific coding standard, e.g., 'no magic numbers']` across our `[language]` codebase. Provide a strategy to identify existing violations and integrate automated checks into our CI/CD pipeline, referencing the relevant [Coding Style Guide](../../docs/style_guides/coding/[language]_style_guide.md)."

## 3. Team Guidance & Mentorship

### 3.1 Onboarding New Developers

**Prompt**: "A new developer is joining the `[team name]` team. Outline a structured onboarding plan for them, focusing on familiarizing them with our [AI Dev Flow Complete Guide](../../AIDevFlow_Complete_Guide.md), [Git SOPs](../Git/), and core codebase. Suggest initial tasks and learning resources."

### 3.2 Resolving Technical Blockers

**Prompt**: "Our team is blocked on `[problem description]` related to `[technology/component]`. Provide potential solutions, debugging strategies, and alternative approaches to unblock the team."

## 4. Project Management Support

### 4.1 Effort Estimation Review

**Prompt**: "Review the following effort estimates for `[tasks/features]` provided by the development team. Based on my knowledge of the codebase and team capabilities, are these estimates realistic? If not, suggest adjustments and provide justification."

### 4.2 Risk Identification

**Prompt**: "During the `[Phase Name]` phase, what are the key technical risks we should be aware of for `[project/feature name]`? Suggest potential mitigation strategies, referencing the [Risk Management SOP](../../docs/SOPs/risk_management_sop.md)."
