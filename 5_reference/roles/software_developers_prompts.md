# AI Prompts for Software Developers

This document provides AI prompts tailored for Software Developers to assist with coding, testing, debugging, and adhering to development standards, aligning with the project's SOPs and best practices.

## 1. Coding & Implementation

### 1.1 Generating Code from Design

**Prompt**: "I need to implement the `[Component/Function Name]` as described in the `[Implementation Guide ID]` (`[path/to/implementation_guide.xml]`). Given the relevant requirements from `[path/to/requirements.xml]` and design from `[path/to/design.md]`, generate the `[language]` code for this component. Ensure it adheres to the [Coding Style Guide](../../docs/style_guides/coding/[language]_style_guide.md) and includes necessary error handling and logging."

### 1.2 Refactoring Code

**Prompt**: "Review the following `[language]` code snippet. Suggest ways to refactor it to improve readability, maintainability, and performance, adhering to [SOLID Principles](../../1_principles/1.4_implementation_principles.md#51-solid-principles) and our [Coding Style Guide](../../docs/style_guides/coding/[language]_style_guide.md).
```[language]
[code snippet]
```"

### 1.3 Implementing Security Best Practices

**Prompt**: "I'm implementing `[Feature Name]` which involves `[sensitive operation, e.g., user authentication, data storage]`. What are the key security best practices I should apply in `[language]` to prevent common vulnerabilities like `[e.g., SQL injection, XSS]`? Reference our [Enterprise Security Best Practices](../../docs/enterprise_security_best_practices.md) and relevant language-specific guides."

## 2. Testing

### 2.1 Writing Unit Tests

**Prompt**: "I've implemented the `[Component/Function Name]` in `[file path]`. Using the [Unit Test Principles](../../1_principles/1.5_unit_test_principles.md) and the [Unit Test XML Template](../../2_templates/2.5_unit_test.xml), generate comprehensive unit tests for this component. Focus on positive, negative, and edge cases, and include appropriate mocks and assertions. The component's code is: 
```[language]
[code snippet]
```"

### 2.2 Creating Test Data

**Prompt**: "I need test data for `[Component/Feature Name]` to cover `[scenario, e.g., valid user creation, invalid email, boundary conditions]`. Generate `[format, e.g., JSON, XML, SQL]` test data that includes `[specific data requirements, e.g., minimum fields, maximum length values, special characters]`."

## 3. Debugging & Troubleshooting

### 3.1 Debugging Code

**Prompt**: "I'm encountering an unexpected behavior in `[Component/Function Name]` when `[describe scenario]`. The input is `[input data]` and the output is `[actual output]`. The expected output was `[expected output]`. What are the common causes for this type of issue, and what debugging steps should I take?"

### 3.2 Interpreting Error Messages

**Prompt**: "I received the following error message in my `[language/framework]` application: `[Error Message]`. What does this error typically indicate, and what are the first steps I should take to investigate and resolve it?"

## 4. Git & Version Control

### 4.1 Performing Git Operations

**Prompt**: "I need to `[perform Git operation, e.g., rebase my feature branch onto develop, cherry-pick a commit, revert a series of commits]`. What are the correct Git commands and best practices for this, referencing our [Git Workflow and Best Practices SOP](../git_sop.md)?"

### 4.2 Managing Work-in-Progress (WIP)

**Prompt**: "I have incomplete work on my current branch `[branch-name]` and need to switch tasks or back up my work. How can I safely manage this WIP using `git stash` or by pushing to a remote personal branch, as per the [Git Work-in-Progress (WIP) Remote Backup SOP](../git_wip_remote_backup_sop.md)? Provide the Git commands."
