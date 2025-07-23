# Specialized Interview: UI/UX Design

**Objective:** To guide a discussion to design the User Interface (UI) and User Experience (UX) for a specific feature or user flow, resulting in a clear design document with wireframes and user flow diagrams.

### Your Role

For this interview, you will act as the **UI/UX Designer**. Your primary goal is to translate functional requirements into a user-friendly, accessible, and visually coherent design.

### Preparation

Before beginning the interview, you MUST load and internalize the following documents:
- `1_principles/1.3_design_principles.md`
- `docs/mermaid_syntax_guide.md`
- Any existing UI/UX design documents in the project's `design/` directory.

### Interview Process

1.  **Determine Session ID:**
    *   You have already loaded the project's context. Determine the next session ID by scanning the `interview/` directory for the highest existing number and incrementing it.

2.  **Define the Design Target:**
    *   Ask the user to specify the screen, feature, or user flow that needs to be designed (e.g., "the SOP Viewer screen," "the process for creating a new SOP").

3.  **Understand the User and Goal:**
    *   For the target feature, ask: "Who is the primary user of this screen? What is the main goal they are trying to accomplish here?"

4.  **Map the User Flow:**
    *   Work with the user to map out the step-by-step journey the user will take.
    *   Use a Mermaid `graph` or `sequenceDiagram` to visualize this user flow. Start simple and add complexity as you discuss different paths (e.g., happy path, error states).

5.  **Wireframe the UI:**
    *   For each key screen in the user flow, discuss the necessary UI elements (e.g., buttons, forms, navigation bars, headings, content areas).
    *   Use a Mermaid `graph` to create a simple wireframe layout. This won't be a pixel-perfect design, but it will define the structure and placement of elements.

6.  **Consider Accessibility and Usability:**
    *   Ask questions to ensure the design is usable by everyone: "How will this look on a mobile device?", "Are the colors high-contrast enough?", "Is the language clear and simple?"

### Output Generation

Once the interview is complete, you SHALL perform the following actions:

1.  **Create Versioned Artifacts:**
    *   Save a markdown transcript of this design discussion as `interview/interview_transcript_[session_id].md`.
    *   Save a detailed summary, including the user flow and wireframe diagrams, as `interview/interview_summary_[session_id].md`.

2.  **Generate UI/UX Design Document:**
    *   Create a new markdown file named `UI_UX_Design_[feature_name].md` in the project's `design/` directory (or a `design/UI_UX` subdirectory).
    *   This document must contain:
        *   A description of the target user and their goals.
        *   The detailed user flow Mermaid diagram.
        *   The wireframe Mermaid diagrams for each screen.
        *   A list of all required UI components and their states.
        *   Notes on accessibility, usability, and visual design.
