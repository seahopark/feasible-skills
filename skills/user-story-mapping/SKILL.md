---
name: user-story-mapping
argument-hint: "[product or workflow]"
description: Create a user story map that lays out activities, steps, tasks, and release slices. Use when planning a workflow, backlog, or MVP around the user journey.
intent: >-
  Visualize the user journey by creating a hierarchical map that breaks down high-level activities into steps and tasks, organized left-to-right as a narrative flow. Use this to build shared understanding across product, design, and engineering, prioritize features based on user workflows, and identify gaps or opportunities in the user experience.
source: https://github.com/deanpeters/Product-Manager-Skills/blob/main/skills/user-story-mapping/SKILL.md
type: component
---

## Purpose

Visualize the user journey by creating a hierarchical map that breaks down high-level activities into steps and tasks, organized left-to-right as a narrative flow.

This is not a backlog — it's a strategic artifact that shows *how* users accomplish their goals, which then informs *what* to build.

## Input

**Works best with:** The product or user workflow being mapped.
**Also useful:** The primary user, the end-to-end narrative as you understand it, existing backlog items to place, and release goals.

**Example invocation:** `Story map for our expense-reporting flow, from receipt capture to reimbursement, with an MVP slice for the pilot.`

---

## Key Concepts

### The Jeff Patton Story Mapping Framework

**Horizontal axis (left-to-right):** User journey over time
- **Backbone:** High-level activities the user performs (3-5)
- **Steps:** Specific actions within each activity (3-5 each)
- **Tasks:** Detailed work required to complete each step (5-7 each)

**Vertical axis (top-to-bottom):** Priority and releases
- **Top rows:** Essential tasks (MVP / Release 1)
- **Lower rows:** Nice-to-have tasks (Future releases)

### Story Map Structure

```
Segment → Persona → Narrative (User's goal)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Activity 1] → [Activity 2] → [Activity 3] → [Activity 4] → [Activity 5]
     ↓              ↓              ↓              ↓              ↓
  [Step 1.1]     [Step 2.1]     [Step 3.1]     [Step 4.1]     [Step 5.1]
  [Step 1.2]     [Step 2.2]     [Step 3.2]     [Step 4.2]     [Step 5.2]
     ↓              ↓              ↓              ↓              ↓
  [Task 1.1.1]   [Task 2.1.1]   [Task 3.1.1]   [Task 4.1.1]   [Task 5.1.1]
  [Task 1.1.2]   [Task 2.1.2]   [Task 3.1.2]   [Task 4.1.2]   [Task 5.1.2]
━━━━━━━━━━━━━━ Release 1 (MVP) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  [Task 1.1.3]   [Task 2.1.3]   ...
━━━━━━━━━━━━━━ Release 2 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ...
```

### Anti-Patterns

- **Not a backlog**: an Activity is user behavior, not a feature the product provides
- **Not a Gantt chart**: this visualizes the user journey, not a project timeline
- **Not static**: keep updating it continuously as you learn more about your users

---

## Application

### Step 1: Define Segment & Persona

```markdown
### Segment:
- [The target segment. Example: "IR staff at asset managers with 50+ employees"]

### Persona:
- [Name, role, main pain points, goals]
```

**Check:** name a specific role/function, not just "the user"

---

### Step 2: Define the Narrative

Write what the user is trying to accomplish as a Jobs-to-be-Done statement:

```markdown
### Narrative:
- [One sentence. Example: "Receive a DDQ, complete it accurately and quickly, and submit it to institutional investors"]
```

**Check:** write it as the outcome the user wants, not "uses the product"

---

### Step 3: Define Activities (Backbone)

3-5 high-level Activities, framed around what the user *does*:

```markdown
### Activities:
1. [Activity 1]
2. [Activity 2]
3. [Activity 3]
4. [Activity 4]
5. [Activity 5, optional]
```

**Check:** ordered left-to-right, framed around user behavior

---

### Step 4: Define Steps

Break each Activity into 3-5 Steps:

```markdown
### Steps:

**Activity 1: [name]**
- Step 1:
- Step 2:
- Step 3:

**Activity 2: [name]**
- Step 1:
...
```

**Check:** observable behavior, in logical order

---

### Step 5: Define Tasks

Break each Step into 5-7 Tasks:

```markdown
### Tasks:

**Activity 1 / Step 1: [name]**
- Task 1:
- Task 2:
- Task 3:
```

**Check:** units that can actually be built

---

### Step 6: Define Release Slices

Cut releases with horizontal lines:

```markdown
### Release Slices:

**Release 1 (MVP):**
- [List of included Tasks — core flow only]

**Release 2:**
- [Additional Tasks — collaboration/automation, etc.]

**Release 3 (Growth):**
- [Expansion features]
```

**MVP criterion:** "Can the user reach their goal with only the Tasks above this line?"

---

## Output Format (Markdown)

Save the result in this structure:

```markdown
# [Product name] User Story Map

## Context
- **Segment:** ...
- **Persona:** ...
- **Narrative:** ...

## Backbone (Activities)
| Activity 1 | Activity 2 | Activity 3 | Activity 4 | Activity 5 |
|---|---|---|---|---|

## Story Map

### Activity 1: [name]
#### Steps & Tasks
| Step | MVP Tasks | R2 Tasks | R3 Tasks |
|---|---|---|---|

...

## Non-Functional Requirements (technical requirements)
Keep as a separate file: `technical-requirements.md`

## Update Log
- YYYY-MM-DD: Initial draft (hypothesis-based)
- YYYY-MM-DD: Updated after team workshop
```
