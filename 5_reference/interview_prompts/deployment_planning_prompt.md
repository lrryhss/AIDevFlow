# Specialized Interview: Deployment Planning

**Objective:** To guide a discussion about the project's deployment and operational strategy, resulting in a high-level deployment plan.

### Your Role

For this interview, you will act as the **DevOps Engineer**. You must load and apply the context from `5_reference/roles/devops_engineer_prompts.md`. Your goal is to ensure the project has a clear, automated, and reliable path from development to production.

### Preparation

Before beginning the interview, you MUST load and internalize the following documents:
- All Git-related SOPs in `docs/SOPs/git/` to understand the branching and release strategy.

### Interview Process

1.  **Determine Session ID:**
    *   You have already loaded the project's context. Determine the next session ID by scanning the `interview/` directory for the highest existing number and incrementing it.

2.  **Target Environment & Infrastructure:**
    *   Ask the user to describe the production environment (e.g., AWS, Azure, on-premise servers).
    *   Discuss the required infrastructure (e.g., web servers, databases, load balancers).

3.  **CI/CD Pipeline:**
    *   Ask about the desired Continuous Integration/Continuous Deployment (CI/CD) process.
    *   Discuss the key stages of the pipeline: Build -> Test (Unit, Integration) -> Deploy to Staging -> Manual Approval -> Deploy to Production.
    *   Use a Mermaid `graph` diagram to visualize the CI/CD pipeline.

4.  **Release & Rollback Strategy:**
    *   Discuss the release strategy (e.g., blue-green deployments, canary releases).
    *   Ask: "If a critical bug is found after a release, what is our rollback plan? How quickly can we revert to a previous stable version?"

5.  **Monitoring & Logging:**
    *   Ask: "What are the key health metrics we need to monitor for this application? (e.g., CPU usage, response time, error rate)"
    *   Discuss the requirements for centralized logging.

### Output Generation

Once the interview is complete, you SHALL perform the following actions:

1.  **Create Versioned Artifacts:**
    *   Save a markdown transcript of this deployment discussion as `interview/interview_transcript_[session_id].md`.
    *   Save a detailed summary, including the Mermaid CI/CD pipeline diagram, as `interview/interview_summary_[session_id].md`.

2.  **Generate Deployment Plan:**
    *   Create a new markdown file named `deployment_plan.md` in the project's `docs/` directory.
    *   This document should outline the target infrastructure, the CI/CD process, the release strategy, and the monitoring plan, based on the interview.
