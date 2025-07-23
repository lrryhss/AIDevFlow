# Standard Operating Procedure: Wireframing

**Version:** 1.0  
**Date:** 2025-07-22  
**Owner:** Head of UI/UX  
**Status:** Approved  

## 1. Purpose

To establish a consistent and efficient process for creating low-fidelity wireframes. Wireframes are the skeletal framework of a user interface, created to focus on the structure, layout, information architecture, and core functionality without the distraction of visual design elements like color, typography, or imagery.

## 2. Scope

This SOP applies to the initial design phase of any new feature, screen, or user flow. Wireframing occurs after initial user stories are defined but before high-fidelity mockups or prototypes are created.

## 3. Roles and Responsibilities

- **UI/UX Designer:** Leads the wireframing process, creates the wireframes, and facilitates the review.
- **Product Manager:** Provides input on requirements and user stories, and validates that the wireframes meet business goals.
- **Lead Developer:** Consults on technical feasibility and constraints during the review process.

## 4. Key Principles

- **Low-Fidelity First:** Wireframes must be grayscale and use generic fonts. The goal is to focus on structure, not aesthetics.
- **Content is King:** Use placeholder text (Lorem Ipsum) for paragraphs, but use realistic text for headings, buttons, and labels to ensure clarity.
- **Structure over Style:** Concentrate on the placement of elements, the hierarchy of information, and the flow between screens.
- **Speed and Iteration:** Wireframes should be quick to create and easy to discard. The process should encourage rapid iteration based on feedback.

## 5. Procedure

### 5.1 Phase 1: Preparation

1.  **Gather Inputs:** The UI/UX Designer gathers the relevant user stories, user personas, and requirements documents.
2.  **Define the Scope:** Clearly identify the screens and user flows that need to be wireframed for the specific feature or epic.
3.  **Choose a Tool:** Use the approved tool for wireframing (e.g., Balsamiq, Figma with a wireframe kit, or even pen and paper for initial sketches).

### 5.2 Phase 2: Creation

1.  **Sketch Initial Ideas:** The designer may start with rough sketches to explore different layouts quickly.
2.  **Create Digital Wireframes:** The designer translates the best ideas into digital wireframes, adhering to the following:
    -   Use only shades of gray, black, and white.
    -   Use simple shapes (rectangles, circles) to represent UI elements.
    -   Clearly label all interactive elements (buttons, links, inputs).
    -   Use image placeholders (e.g., a box with an X) to denote where images will go.
    -   Create a wireframe for each distinct state of the interface (e.g., empty state, loading state, error state, ideal state).
3.  **Annotate for Clarity:** Add notes and annotations directly on the wireframes to explain functionality, interactions, or user flows that are not immediately obvious from the static design.

### 5.3 Phase 3: Review and Iteration

1.  **Internal Review:** The UI/UX Designer conducts an informal review with the Product Manager and Lead Developer.
2.  **Gather Feedback:** The focus of the feedback should be on:
    -   Does this meet the user's goals?
    -   Is the flow logical and efficient?
    -   Are there any missing elements or states?
    -   Are there any obvious technical red flags?
3.  **Iterate:** The designer updates the wireframes based on the feedback. This cycle may repeat 2-3 times.

### 5.4 Phase 4: Finalization and Handoff

1.  **Approval:** The Product Manager gives final approval on the wireframes.
2.  **Link to Documentation:** The approved wireframes are exported or linked within the relevant design document (e.g., `05_UI_UX_Design.md`).
3.  **Proceed to Mockups:** Once approved, the wireframes serve as the blueprint for the high-fidelity mockups.

## 6. Wireframe Checklist

Before submitting for review, ensure the wireframe includes:

- [ ] A clear title for each screen.
- [ ] All primary navigation and interactive elements.
- [ ] Key content areas with placeholder text/images.
- [ ] All form fields, buttons, and links.
- [ ] Annotations for complex interactions.
- [ ] Representation of different states (e.g., modals, pop-ups, errors).
- [ ] Adherence to a grid system for layout consistency.

## Appendix A: Wireframing with ASCII Art

For ultimate portability and zero dependencies, ASCII art can be used to create text-based wireframes directly inside a Markdown code block. This method is excellent for version control and ensures the wireframe can be viewed on any system capable of displaying monospaced text.

### Key Concepts

- **Code Blocks:** All ASCII wireframes must be enclosed in a Markdown code block (```) to preserve spacing and alignment.
- **Characters for Structure:** Use characters like `+`, `-`, and `|` to create boxes and lines.
- **Characters for Content:** Use `#` for headings, `[]` for input fields, and `()` for buttons.

### Example 1: Simple Login Form Wireframe

This example shows a basic login form.

```
+----------------------------------------+
|                                        |
|  ## Welcome Back!                      |
|                                        |
|  Email Address                         |
|  [email@example.com               ]  |
|                                        |
|  Password                              |
|  [••••••••••••••••••              ]  |
|                                        |
|  +----------------------------------+  |
|  |             (Login)              |  |
|  +----------------------------------+  |
|                                        |
|  Forgot Password? | Sign Up           |
|                                        |
+----------------------------------------+
```

### Example 2: Article Card Wireframe

This example demonstrates a component with an image, text, and actions.

```
+----------------------------------------------------+
| +------------------------------------------------+ |
| |                                                | |
| |                IMAGE (16:9)                    | |
| |                                                | |
| +------------------------------------------------+ |
|                                                    |
|  ### Article Title Goes Here                       |
|                                                    |
|  Lorem ipsum dolor sit amet, consectetur           |
|  adipiscing elit. Sed do eiusmod tempor...         |
|                                                    |
|  +-------------+   +-------------+                 |
|  | (Read More) |   |   (Share)   |                 |
|  +-------------+   +-------------+                 |
|                                                    |
+----------------------------------------------------+

```

### Best Practices for ASCII Art Wireframing

- **Use a Monospaced Font:** When creating or viewing these wireframes, use a monospaced font (like Courier, Monaco, or Consolas) to ensure proper alignment.
- **Keep it Simple:** This technique is for low-fidelity structures. Focus on layout and element placement, not on creating perfect visual representations.
- **Be Patient:** Creating ASCII art can be tedious. Use copy-paste for repeating patterns like box lines.
- **Add Annotations:** Use numbered annotations inside or outside the wireframe and explain them in the text below for complex interactions.

**Annotation Example:**

```
+----------------------------------+
|  [Search...                ] (1) |
+----------------------------------+
```

1.  **Search:** As the user types, results appear in a dropdown below this field.
