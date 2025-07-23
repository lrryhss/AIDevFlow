# Standard Operating Procedure: UI/UX Usability Review

**Version:** 1.0  
**Date:** 2025-07-22  
**Owner:** Head of UI/UX  
**Status:** Approved  

## 1. Purpose

To provide a systematic process for conducting expert usability reviews (also known as heuristic evaluations) on user interfaces. This SOP ensures that potential usability issues are identified and addressed consistently and efficiently during the design and development lifecycle, reducing the cost of fixing problems later.

## 2. Scope

This SOP applies to all new features and significant redesigns of existing features for all software products. It should be performed before user acceptance testing (UAT) and, ideally, before development begins on a high-fidelity prototype.

## 3. Roles and Responsibilities

- **UI/UX Designer (Lead Reviewer):** Responsible for initiating, organizing, and leading the review process. Consolidates findings and presents the final report.
- **Product Manager:** Participates in the review to provide context on user goals and business requirements. Helps prioritize findings.
- **Lead Developer/Engineer:** Participates in the review to provide technical feasibility insights on proposed solutions.
- **Reviewers (Optional):** 1-2 other team members (designers, developers, QA) who can provide a fresh perspective.

## 4. Heuristics

This review process is based on **Jakob Nielsen's 10 Usability Heuristics for User Interface Design**. All reviewers must be familiar with these principles:

1.  **Visibility of system status:** Keep users informed.
2.  **Match between system and the real world:** Speak the user's language.
3.  **User control and freedom:** Provide a clear "emergency exit."
4.  **Consistency and standards:** Don't make users guess.
5.  **Error prevention:** Design to prevent problems.
6.  **Recognition rather than recall:** Make elements and options visible.
7.  **Flexibility and efficiency of use:** Provide accelerators for experts.
8.  **Aesthetic and minimalist design:** Don't clutter with irrelevant information.
9.  **Help users recognize, diagnose, and recover from errors:** Use plain language for errors.
10. **Help and documentation:** Provide easy-to-search help.

## 5. Procedure

### 5.1 Phase 1: Preparation (1-2 hours)

1.  **Define Scope:** The Lead Reviewer clearly defines the UI area or user flow to be reviewed (e.g., "User registration and login flow," "Shopping cart and checkout process").
2.  **Gather Materials:** The Lead Reviewer collects all necessary artifacts:
    -   Link to the Figma prototype, staging environment, or relevant designs.
    -   Relevant user personas to anchor the review in user needs.
    -   A list of specific tasks to be evaluated (e.g., "Task: Add an item to the cart").
3.  **Schedule Review Session:** The Lead Reviewer schedules a 60-90 minute meeting with all participants.
4.  **Create a Findings Log:** The Lead Reviewer sets up a shared document (e.g., a spreadsheet or Confluence page) for logging issues with the following columns:
    -   `Issue ID`
    -   `Location (Screen/Component)`
    -   `Description of Issue`
    -   `Heuristic(s) Violated`
    -   `Severity (1-4)`
    -   `Proposed Solution`
    -   `Notes`

### 5.2 Phase 2: Execution (Individual Review - 1 hour per reviewer)

1.  **Individual Evaluation:** Before the group session, each reviewer independently goes through the defined user flows and tasks.
2.  **Log Findings:** As reviewers identify potential issues, they log them in the shared findings document. This prevents groupthink in the initial pass.

### 5.3 Phase 3: Group Review and Consolidation (1-1.5 hours)

1.  **Consolidate Findings:** The Lead Reviewer facilitates the scheduled meeting, going through the logged findings one by one.
2.  **Discuss and Refine:** The group discusses each issue to confirm its validity and agree on the violated heuristics.
3.  **Assign Severity:** For each confirmed issue, the group assigns a severity rating.

    | Severity | Rating | Description |
    |----------|--------|-------------|
    | **Critical** | 4 | A usability catastrophe. Prevents users from completing a primary task. Must be fixed before release. |
    | **Major** | 3 | A significant issue that will cause frustration and may lead to task failure. High priority to fix. |
    | **Minor** | 2 | A noticeable issue that may cause some irritation but will not prevent task completion. Fix if time allows. |
    | **Trivial** | 1 | A cosmetic or low-impact issue. May be noted but not prioritized. |

4.  **Brainstorm Solutions:** The group briefly brainstorms potential solutions for high-priority issues.

### 5.4 Phase 4: Reporting and Follow-up (1-2 hours)

1.  **Create Summary Report:** The Lead Reviewer creates a concise report summarizing the key findings. The report should include:
    -   An executive summary of the overall usability health.
    -   A prioritized list of the top 5-10 most critical issues.
    -   Visuals (screenshots) illustrating the key problems.
    -   A link to the full findings log.
2.  **Share Report:** The report is shared with all project stakeholders.
3.  **Create Tickets:** The Product Manager and Lead Reviewer work together to translate the prioritized findings into actionable tickets in the project management tool (e.g., Jira, Asana), assigning them to the appropriate backlog or sprint.

## 6. Tools and References

-   **Design/Prototyping Tool:** Figma, Sketch, Adobe XD
-   **Project Management:** Jira, Asana, Trello
-   **Documentation:** Confluence, Notion, Google Docs
-   **Reference:** [Nielsen's 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
