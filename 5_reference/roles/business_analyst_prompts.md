# AI Prompts for Business Analysts

This document provides AI prompts tailored for Business Analysts to assist with requirements gathering, stakeholder management, and ensuring alignment between business needs and technical solutions, adhering to the project's SOPs and best practices.

## 1. Requirements Discovery & Analysis

### 1.1 Generating User Stories from Interview Notes

**Prompt**: "Based on the following discussion notes from a stakeholder interview, and applying the principles from [User Story Principles](../../1_principles/1.1_user_story_principles.md), please generate a user story using the template found in [User Story Markdown Template](../../2_templates/2.1_user_story.md). Focus on the 'As a..., I want..., So that...' format and include initial acceptance criteria.

**Discussion Notes:**
```
[Interview notes/transcripts]
```"

### 1.2 Consolidating User Stories into Requirements

**Prompt**: "I have a set of user stories related to `[Feature Name]` (assume these are available in `[path/to/user_stories_folder]`). Using the [Requirements Writing Style Guide](../../docs/requirements_writing_style_guide.md) and the [Requirement XML Template](../../2_templates/2.3_requirement.xml), create a complete set of detailed functional and non-functional requirements. Ensure all edge cases and error conditions implied by the stories are covered, and include clear acceptance criteria."

### 1.3 Defining Non-Functional Requirements (NFRs)

**Prompt**: "For the `[Module/System Name]`, what are the typical non-functional requirements (e.g., performance, security, scalability, usability) that we should consider? Provide examples and metrics, referencing the [Requirements Writing Style Guide](../../docs/requirements_writing_style_guide.md)."

## 2. Stakeholder Management

### 2.1 Drafting Communication Plans

**Prompt**: "Draft a communication plan for the `[Project Name]` project, targeting `[Stakeholder Group, e.g., Executive Leadership, Development Team, End Users]`. Include suggested communication channels, frequency of updates, and key information to share, referencing the [Phase 1: Requirements Discovery and Analysis SOP](../../docs/SOPs/phase_1_requirements_discovery_analysis_sop.md)."

### 2.2 Facilitating Requirements Workshops

**Prompt**: "Prepare an agenda for a 2-hour requirements gathering workshop for the `[Feature Name]` feature. Include activities for eliciting business needs, identifying user roles, and defining initial scope. Suggest techniques for encouraging participation and managing discussions."

## 3. Design & Implementation Validation

### 3.1 Validating Design Against Requirements

**Prompt**: "Review the `[Design Document Name]` (`[path/to/design_doc.md]`) against the approved `[Requirements Document Name]` (`[path/to/requirements_doc.xml]`). Identify any gaps, inconsistencies, or areas where the design does not fully meet the specified requirements."

### 3.2 Assisting with User Acceptance Testing (UAT)

**Prompt**: "We are planning UAT for the `[Feature Name]` feature. Based on the [Phase 3: Implementation and Testing SOP](../../docs/SOPs/phase_3_implementation_testing_sop.md), help me define the UAT scope, create test scenarios from the user stories, and outline the success criteria for user acceptance."

## 4. Risk Management

### 4.1 Identifying Business Risks

**Prompt**: "For the `[Project Name]` project, what are the potential business risks related to `[specific area, e.g., scope creep, user adoption, market changes]`? Suggest initial mitigation strategies, referencing the [Risk Management SOP](../../docs/SOPs/risk_management_sop.md)."
