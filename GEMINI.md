# Gemini Project Context: AIDevFlow

This file provides the context for the "AI Dev Flow" project, a structured methodology for software development.

## Project Overview

The "AI Dev Flow" is a comprehensive framework designed to guide the development of enterprise-grade software from initial idea to final implementation and testing. It emphasizes a systematic, AI-assisted approach to ensure consistency, quality, and traceability throughout the development lifecycle. The process is broken down into five main stages:

1.  **Principles**: Foundational concepts for development.
2.  **Templates**: Standardized formats for user stories, requirements, etc.
3.  **Guides**: Step-by-step instructions for the development process.
4.  **Examples**: Concrete illustrations of the templates and guides.
5.  **Reference**: AI prompts and other quick-reference materials.

## Key Concepts & Terminology

*   **User Stories**: The starting point for defining features, focusing on user needs and value. They follow the "As a..., I want..., So that..." format.
*   **Requirements**: Formal, detailed specifications derived from user stories. They are categorized as Functional, Non-Functional, and Technical.
*   **Implementation Guides**: Step-by-step instructions for developers (human or AI) to build components based on the design documents.
*   **Unit Tests**: The foundation of quality assurance, defined in XML templates to ensure comprehensive coverage of requirements.
*   **Traceability**: A core principle, ensuring that every piece of code and every test can be traced back to a specific requirement and user story. This is managed through explicit linking in the XML documents.
*   **SOP (Standard Operating Procedure)**: The `docs/enterprise_software_sop.md` file outlines the formal, phase-based process for enterprise projects, including roles, responsibilities, and quality gates.

## File Structure and Naming Conventions

The project is organized into numbered directories representing the stages of the development flow:

*   `1_principles/`
*   `2_templates/`
*   `3_guides/`
*   `4_examples/`
*   `5_reference/`
*   `docs/`: Contains high-level guides and standards, such as the enterprise coding style guide and SOPs.

Within these, files are named descriptively (e.g., `1.1_user_story_principles.md`).

## Core Technologies & Formats

*   **Markdown (`.md`)**: Used for guides, principles, and human-readable documentation. It often includes Mermaid diagrams for visualization.
*   **XML (`.xml`)**: Used for structured data such as templates for user stories, requirements, implementation guides, and unit tests. This structured format is ideal for AI processing and validation.

## Development Workflow

The prescribed workflow is sequential and traceable:

1.  **Idea to User Stories**: An interview/brainstorming process is used to generate user stories from an initial idea.
2.  **User Stories to Requirements**: User stories are analyzed and consolidated into formal, detailed requirements documents.
3.  **Requirements to Design**: The requirements are used to create comprehensive design documents (architecture, API specs, data models).
4.  **Design to Implementation**: The design documents are translated into step-by-step implementation guides.
5.  **Implementation and Testing**: Developers follow the implementation guides to write code, and unit tests are created to verify that the implementation meets the requirements.

## AI Integration

The entire process is designed to be augmented by AI. The `5_reference/5.1_ai_prompts.md` file contains specific prompts for generating artifacts at each stage, such as creating user stories from interview notes, generating requirements from stories, and creating implementation guides from design documents.

## Coding Style and Standards

The `docs/enterprise_coding_style_guide.md` provides a comprehensive guide for writing code. Key principles include:

*   **Readability and Consistency**: Code should be easy to understand and follow established conventions.
*   **SOLID Principles**: Adherence to Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.
*   **Language-Specific Conventions**: Detailed naming and structure guidelines for Java/C#, Python, and JavaScript/TypeScript.
*   **Documentation**: JSDoc, Javadoc, or docstrings are required for public APIs, and file headers should trace back to requirements.
*   **Security**: Emphasis on input validation, secure coding patterns, and proper secret management.

## Requirements Writing Style

The `docs/requirements_writing_style_guide.md` establishes standards for writing requirements, emphasizing:

*   **Clarity and Precision**: Using specific, measurable terms and avoiding ambiguity.
*   **Modal Verbs**: Using "SHALL" for mandatory requirements, "SHOULD" for recommendations, and "MAY" for optional features.
*   **Active Voice and Present Tense**.

## My Expertise as a Software Architect

Leveraging years of experience as a software architect, I possess deep expertise in requirements gathering. This includes facilitating stakeholder workshops, eliciting detailed business and technical requirements, defining clear acceptance criteria, and ensuring comprehensive traceability from initial user stories to final system specifications. My approach emphasizes clarity, precision, and alignment with business objectives to lay a solid foundation for successful software development.

This summary will guide my actions and ensure that any contributions I make are consistent with the project's established standards and workflow.

## Mermaid Diagram Fixes (2025-07-20)

**Issue**: Parsing errors in Mermaid diagrams due to specific syntax interpretations, particularly with parentheses `()` within node labels and text within curly braces `{}`.

**Resolution**: When creating Mermaid diagrams, avoid these common parsing issues by:
- Enclosing text within node labels that contain special characters (like parentheses `()`) in square brackets `[]` or double quotes `""` (e.g., `[Node Text (with parens)]` or `{"Node Text (with parens)"}`).
- Ensuring text within curly brace nodes `{}` is always enclosed in double quotes `""` (e.g., `{"Decision Text?"}`).
- Strictly adhering to the guidelines outlined in `1_principles/1.3_design_principles.md`, particularly Section 4: "Mermaid Diagram Guidelines."

**Affected Files**: All SOPs in `docs/SOPs/` and `docs/SOPs/Git/` containing Mermaid diagrams.

## Git Standard Operating Procedures (SOPs) (2025-07-20)

**Description**: A new set of SOPs has been created to standardize Git workflow and best practices, particularly focusing on the GitFlow branching model. These documents provide detailed guidance for developers on various Git operations.

**Location**: `docs/SOPs/Git/`

**Included SOPs**:
- GitFlow Overview
- GitFlow Feature Branch Workflow
- GitFlow Release Branch Workflow
- GitFlow Hotfix Branch Workflow
- Git Merge Conflict Resolution
- Git Work-in-Progress (WIP) Remote Backup