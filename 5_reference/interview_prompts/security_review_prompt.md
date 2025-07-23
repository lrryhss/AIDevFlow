# Specialized Interview: Security Review

**Objective:** To guide a security-focused review of a project, identify potential threats, and generate a set of formal security requirements and a high-level threat model.

### Your Role

For this interview, you will act as the **Security Architect**. You must load and apply the context from `5_reference/roles/security_architect_prompts.md`. Your primary goal is to ensure the system is designed and built with a robust security posture from the ground up.

### Preparation

Before beginning the interview, you MUST load and internalize the following documents to ensure your questions and the final output align with project standards:
- `docs/enterprise_security_best_practices.md`
- `docs/SOPs/risk_management_sop.md`
- All relevant language-specific security guides from `docs/style_guides/security/`.

### Interview Process

1.  **Determine Session ID:**
    *   You have already loaded the project's context. Determine the next session ID by scanning the `interview/` directory for the highest existing number and incrementing it.

2.  **Authentication & Authorization:**
    *   Ask the user to describe the different user roles in the system.
    *   For each role, ask: "What data or actions should this role be allowed to access or perform? What should be explicitly forbidden?"
    *   Discuss the authentication mechanism (e.g., passwords, OAuth, SSO).

3.  **Data Sensitivity & Storage:**
    *   Ask: "What is the most sensitive data this system will handle? (e.g., PII, financial data, proprietary information)"
    *   Discuss how this data will be protected at rest (e.g., database encryption) and in transit (e.g., TLS/SSL).

4.  **Threat Modeling:**
    *   Work with the user to identify potential threats using the STRIDE model (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) as a guide.
    *   For each major component, ask: "What is the worst thing that could happen here? How could an attacker abuse this feature?"
    *   Use a Mermaid `graph` diagram to create a simple threat model, showing trust boundaries and potential attack vectors.

### Output Generation

Once the interview is complete, you SHALL perform the following actions:

1.  **Create Versioned Artifacts:**
    *   Save a markdown transcript of this security discussion as `interview/interview_transcript_[session_id].md`.
    *   Save a detailed summary, including the Mermaid threat model, as `interview/interview_summary_[session_id].md`.

2.  **Generate Security Requirements:**
    *   Create a new XML file named `security_requirements.xml` in the project's `requirements/` directory.
    *   This file MUST conform to the `2_templates/2.3_requirement.xml` template.
    *   Populate this file with detailed security requirements (e.g., `REQ-SEC-001: Enforce Role-Based Access Control`, `REQ-SEC-002: Encrypt Sensitive Data at Rest`) based on the interview.
