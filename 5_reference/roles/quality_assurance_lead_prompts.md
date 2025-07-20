# AI Prompts for Quality Assurance Leads

This document provides AI prompts tailored for Quality Assurance Leads to assist with defining testing strategies, ensuring quality gates are met, and managing the defect lifecycle, adhering to the project's SOPs and best practices.

## 1. Test Strategy & Planning

### 1.1 Defining Testing Strategy

**Prompt**: "For the `[Feature/Module Name]` based on `[Requirements Document ID]` and `[Design Document ID]`, propose a comprehensive testing strategy. Include recommendations for unit, integration, system, performance, and security testing, referencing the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md)."

### 1.2 Creating Test Plans

**Prompt**: "Draft a test plan for `[Test Type, e.g., System Integration Testing (SIT), User Acceptance Testing (UAT)]` for `[Feature/Module Name]`. Outline the scope, objectives, test environment requirements, entry/exit criteria, and roles/responsibilities, referencing the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md)."

### 1.3 Test Case Generation

**Prompt**: "Given the following user story: 
```markdown
[User Story Content]
```
And the associated requirements: 
```xml
[Requirements XML Content]
```
Generate a set of detailed test cases (including positive, negative, and edge cases) for `[Test Type, e.g., unit, integration, UAT]`. For each test case, specify preconditions, steps, and expected results, referencing the [Unit Test Principles](../../1_principles/1.5_unit_test_principles.md) and [Unit Test XML Template](../../2_templates/2.5_unit_test.xml)."

## 2. Test Execution & Defect Management

### 2.1 Analyzing Test Results

**Prompt**: "Review the attached test execution report for `[Test Cycle Name]` (`[path/to/test_report.xml]`). Summarize the key findings, identify areas of concern, and recommend next steps. Are we meeting our quality gates as defined in the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md)?"

### 2.2 Prioritizing Defects

**Prompt**: "We have the following new defects reported: 
```
[List of defects with severity/impact]
```
Based on our project's defect prioritization guidelines, recommend a priority level for each defect and suggest which team (development, DevOps, etc.) should address it first."

### 2.3 Regression Test Suite Management

**Prompt**: "We've implemented `[Feature Name]` and fixed `[Bug ID]`. What existing test cases should be added to our regression test suite to ensure these changes don't introduce new issues in the future?"

## 3. Quality Gates & Compliance

### 3.1 Ensuring Quality Gate Adherence

**Prompt**: "Our CI/CD pipeline is configured with several quality gates (e.g., code coverage, static analysis, test pass rates). How can I effectively monitor adherence to these gates and what actions should be taken if a gate fails, referencing the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md)?"

### 3.2 Reviewing Test Coverage

**Prompt**: "Analyze the code coverage report for `[Module/Component Name]` (`[path/to/coverage_report]`). Identify areas with low coverage and suggest specific unit tests that should be added to improve it, referencing the [Unit Test Principles](../../1_principles/1.5_unit_test_principles.md)."

## 4. Continuous Improvement

### 4.1 Post-Implementation Retrospective

**Prompt**: "Prepare an agenda for a post-implementation retrospective for the QA team after the `[Project/Release Name]` deployment. Focus on what went well, what could be improved in our testing processes, and actionable items for the next project."
