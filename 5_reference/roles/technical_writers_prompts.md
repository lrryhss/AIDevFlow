# AI Prompts for Technical Writers

This document provides AI prompts tailored for Technical Writers to assist with creating clear, concise, and accurate documentation for software projects, adhering to the project's SOPs and best practices.

## 1. Documentation Creation

### 1.1 Drafting User Manuals

**Prompt**: "Based on the user stories for `[Feature Name]` (`[path/to/user_stories.md]`) and the completed implementation guide (`[path/to/implementation_guide.xml]`), draft a section for the user manual explaining how to `[specific user task, e.g., 'create a new account']`. Focus on clear, step-by-step instructions and user-friendly language. Reference the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."

### 1.2 Creating API Documentation

**Prompt**: "Given the OpenAPI specification for `[API Name]` (`[path/to/api_spec.yaml]`), generate comprehensive API documentation for the `[Endpoint, e.g., POST /users]` endpoint. Include request/response examples, error codes, authentication requirements, and usage examples in `[language, e.g., cURL, Python]`. Reference the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."

### 1.3 Writing Release Notes

**Prompt**: "Draft release notes for `[Version Number]` of `[Application Name]`. Summarize the key user-facing changes, new features, bug fixes, and any migration steps required. Ensure the language is clear and concise, suitable for end-users and stakeholders."

## 2. Documentation Review & Improvement

### 2.1 Reviewing Existing Documentation

**Prompt**: "Review the attached `[Document Name]` (`[path/to/document.md]`) for clarity, accuracy, and completeness. Identify any ambiguities, outdated information, or areas where the language could be improved for a `[Target Audience, e.g., technical users, end-users]`. Reference our [Requirements Writing Style Guide](../../docs/requirements_writing_style_guide.md) for consistency."

### 2.2 Improving Technical Explanations

**Prompt**: "The following technical explanation in `[Document Name]` is unclear: 
```
[text snippet]
```
Rewrite this explanation to be more easily understood by a `[Target Audience, e.g., junior developer, non-technical stakeholder]`, while maintaining technical accuracy."

## 3. Knowledge Transfer

### 3.1 Preparing Training Materials

**Prompt**: "Based on the user manual section for `[Feature Name]`, prepare a short training module (e.g., a presentation outline or a script for a video tutorial) for end-users. Focus on key functionalities and common use cases."

### 3.2 Documenting Troubleshooting Guides

**Prompt**: "For `[Application Name]`, create a troubleshooting guide for `[Common Problem, e.g., 'login failures']`. Include common symptoms, potential causes, and step-by-step solutions for users or support staff. Reference the [Phase 4: Documentation and Knowledge Transfer SOP](../../docs/SOPs/phase_4_documentation_knowledge_transfer_sop.md)."
