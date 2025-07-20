# AI Prompts for Developers

This document provides AI prompts tailored for developers to assist with various tasks throughout the software development lifecycle, adhering to the project's SOPs and best practices.

## 1. Feature Development

### 1.1 Starting a New Feature

**Prompt**: "I'm starting a new feature. Based on the [GitFlow Feature Branch Workflow SOP](../Git/gitflow_feature_sop.md), please provide the Git commands to create and set up my feature branch from `develop`."

### 1.2 Implementing a Task

**Prompt**: "I need to implement the `[Task Name]` from the `[Implementation Guide ID]` implementation guide. Given the relevant requirements from `[path/to/requirements.xml]`, design from `[path/to/design.md]`, and unit tests from `[path/to/unit_tests.xml]`, generate the `[language]` code for the `[component/function]` as described in the guide. Include necessary imports and adhere to the [Coding Style Guide](../../docs/style_guides/coding/[language]_style_guide.md)."

### 1.3 Writing Unit Tests

**Prompt**: "I've implemented the `[component/function]` as per the `[Implementation Guide ID]`. Using the [Unit Test Principles](../../1_principles/1.5_unit_test_principles.md) and the [Unit Test XML Template](../../2_templates/2.5_unit_test.xml), generate comprehensive unit tests for this component. Focus on positive, negative, and edge cases, and include appropriate mocks and assertions. The component's code is: 
```[language]
[code snippet]
```"

### 1.4 Resolving Merge Conflicts

**Prompt**: "I've encountered a merge conflict while pulling changes from `develop` into my feature branch. The conflicting file is `[file path]`. Please explain the conflict markers and guide me through the steps to resolve it using the [Git Merge Conflict Resolution SOP](../Git/gitflow_merge_conflict_sop.md)."

## 2. Code Quality & Review

### 2.1 Adhering to Coding Standards

**Prompt**: "Review the following `[language]` code snippet for adherence to the [Coding Style Guide](../../docs/style_guides/coding/[language]_style_guide.md). Point out any violations and suggest corrections.
```[language]
[code snippet]
```"

### 2.2 Preparing for Code Review

**Prompt**: "I'm about to create a Pull Request for my feature branch `[branch-name]`. Based on the project's code review best practices, what should I check before submitting? Provide a checklist."

## 3. Work-in-Progress (WIP) Management

### 3.1 Backing Up Incomplete Work

**Prompt**: "I have incomplete work on my current branch `[branch-name]` and need to switch tasks. How can I safely back up my work to the remote repository without committing to the main branch, as per the [Git Work-in-Progress (WIP) Remote Backup SOP](../Git/git_wip_remote_backup_sop.md)? Provide the Git commands."

## 4. Troubleshooting

### 4.1 Debugging Assistance

**Prompt**: "I'm encountering an error `[error message]` in `[file:line]`. The relevant code snippet is: 
```[language]
[code snippet]
```
What are the common causes for this error, and what debugging steps should I take?"

### 4.2 Understanding Build Failures

**Prompt**: "My CI/CD pipeline build failed with the following logs: 
```
[build logs]
```
What is the most likely cause of the failure, and how can I fix it?"
