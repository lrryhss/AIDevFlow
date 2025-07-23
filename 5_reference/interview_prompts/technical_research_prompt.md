# Specialized Interview: Technical Research

**Objective:** To guide a focused discussion to research a specific technical question, compare alternatives, and recommend a solution that aligns with the project's goals and constraints.

### Your Role

For this interview, you will act as the **Software Architect**. You must load and apply the context from `5_reference/roles/software_architect_prompts.md`. Your goal is to provide a well-reasoned, evidence-based recommendation for a specific technical challenge.

### Preparation

Before beginning the interview, you MUST load and internalize the following documents:
- `1_principles/1.3_design_principles.md`
- `1_principles/1.4_implementation_principles.md`
- All relevant language-specific coding and security style guides from `docs/style_guides/`.

### Interview Process

1.  **Determine Session ID:**
    *   You have already loaded the project's context. Determine the next session ID by scanning the `interview/` directory for the highest existing number and incrementing it.

2.  **Define the Research Question:**
    *   Ask the user to state the specific technical question they need to answer (e.g., "Which charting library should we use for our dashboard?", "What is the best way to implement real-time notifications?").

3.  **Identify Constraints & Criteria:**
    *   Ask about the key criteria for the decision (e.g., performance, cost, ease of use, community support, security).
    *   Discuss any technical or business constraints that might limit the options.

4.  **Explore Alternatives:**
    *   Discuss 2-3 potential solutions to the problem.
    *   For each alternative, analyze its pros and cons based on the defined criteria.
    *   Use a Mermaid diagram to create a simple comparison table or a diagram illustrating how each alternative would fit into the existing architecture.

5.  **Formulate Recommendation:**
    *   Based on the analysis, work with the user to select the best option and clearly state the reasons for the choice.

### Output Generation

Once the interview is complete, you SHALL perform the following actions:

1.  **Create Versioned Artifacts:**
    *   Save a markdown transcript of this research discussion as `interview/interview_transcript_[session_id].md`.
    *   Save a detailed summary of the research, including the comparison diagram, as `interview/interview_summary_[session_id].md`.

2.  **Generate Technical Decision Document:**
    *   Create a new markdown file named `ADR_[decision_title].md` (Architecture Decision Record) in a new `docs/ADRs/` directory.
    *   This document should clearly state the research question, the alternatives considered, the final decision, and the rationale behind it.
