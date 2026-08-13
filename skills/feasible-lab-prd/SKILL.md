---
name: feasible-lab-prd
description: |
  Feasible Lab PRD draft-writing skill. Describe the intent behind a new feature or project, and it immediately produces a Markdown PRD draft in Feasible Lab's AC-centric format (7 sections + Q&A, flow-and-AC merged together, UI and policy merged per feature).

  The PM defines the WHAT (AC, policy, flow); the engineering team decides the HOW (architecture, API design, DB schema) by reading the AC.

  Use this skill whenever:
  - Someone asks for a PRD or planning draft ("write a PRD", "make a PRD draft", "put together a planning draft")
  - Someone asks you to lay out the spec, flow, and policy for a new feature for the first time
  - Someone asks for a document "in PRD format" or "in Feasible Lab format"
  - A document is needed to kick off the first planning-to-engineering discussion
  - An existing PRD draft section needs to be filled in or updated
---

# Feasible Lab PRD Draft-Writing Skill

## What this skill is for

A Feasible Lab PRD draft is **the starting document engineering can walk straight into a first discussion with**. It isn't a finished spec — it's a draft meant to share planning intent and direction and kick off a conversation.

**Core principle: PM owns WHAT, engineering owns HOW**
- What the PM defines: background/purpose, scope, user flow, Acceptance Criteria, UI composition, policy
- What engineering defines: system architecture, API design, DB schema
- Why: if the PM predefines technical specs, engineers either get dragged into that frame or end up paying the cost of correcting wrong assumptions

---

## Pre-writing interview (always gather this before writing)

Before writing the PRD, gather the information below. If something has already been covered sufficiently in conversation, don't ask again — just use it.

**Must-have items:**
1. **Feature/project name** — used for the PRD title (the Notion page title)
2. **The problem being solved, or the purpose** — used to write Section 1
3. **The main user flow** — used to write Section 3
4. **Figma link** (if any) — linked in the document header and Section 4
5. **Linear project link** (if any) — linked in the document header

If the information is insufficient, **ask before writing**, then write once you have the answer. Don't stop midway to ask.

---

## PRD notation rules

| Notation | Meaning |
|---|---|
| `TBD` | Number/detail not yet finalized |
| `Defined later` | Not in scope for the current build (call it out in the Not Doing list) |

**Prohibited notation:**
- Status markers like ✅ / 🔲 — they make the document heavier and scatter attention
- Pixel/dimension specs — Figma is the source of truth; putting them in the PRD creates dual maintenance
- PlantUML diagrams — write flows as text steps instead

---

## Document header

Written so it can be pasted straight into Notion. Don't write an H1 document title (that goes in the Notion page title field instead).

```markdown
Linear: [link or TBD]
Figma: [link or TBD]
Date: [date] | Author: [name] | Status: Draft v0.1

---
```

---

## PRD document structure (7 sections + Q&A)

Notion heading convention: shift each heading level up by one — `##` (H2) becomes `#` (H1), `###` (H3) becomes `##` (H2).

---

### Section 1. Background

Explain in one line what this feature solves, and compress the reasoning into 3 lines or fewer.

```markdown
# 1. Background

[What this feature solves, in one sentence]

1. [Reason 1]
2. [Reason 2]
3. [Reason 3]

| Target | Description |
|---|---|
| [User type 1] | [Description] |
| [User type 2] | [Description] |
```

---

### Section 2. Scope

**A PRD without a Not Doing list is incomplete.** It's the key mechanism that prevents scope arguments mid-build.

```markdown
# 2. Scope

## Outcome

1. [Goal 1]
2. [Goal 2]

## Output

| Category | Deliverable |
|---|---|
| Concept change | [Description] |
| New feature | [Description] |
| New policy | [Description] |
| UI change | [Description] |

## Not Doing

- [Excluded item 1]
- [Excluded item 2]
```

---

### Section 3. User Flow & Acceptance Criteria

**Write flow and AC together.** Attach the AC directly under each flow to keep the connection visible.

Number flows as `3-1`, `3-2`, etc., matching Linear issues 1:1.

**Incremental principle:** each flow defines only what's newly introduced at that point. Don't fold concepts from a later flow into an earlier flow's AC.

**Flow-writing rules:**
- Write as noun phrases / abbreviated form ("the user clicks" → "click")
- Include error cases inside the User Flow steps
- Exclude technical terms (API calls, DB writes, etc.)

**AC-writing principles:**
- Write as an observable outcome ending in a verb
- One AC = one verifiable condition
- Include error cases and edge cases
- Never include technical implementation details (API, DB)

---

**Section 3 indentation convention**

```
## 3-X. [Flow name]          ← H2: flow title

[Policy/context notes]        ← preconditions or policy for the flow (only if applicable)

### User Flow                 ← H3: user flow

**[Sub-flow name]**           ← Bold: distinguishes a sub-flow (create/edit/delete, bulk/single, etc. — omit if not applicable)

1. [Action]
2. [System response]
   - Success → [Result]
   - Failure → [Error handling]

### Acceptance Criteria       ← H3: acceptance criteria

**[Sub-flow name]**           ← Bold: matches the sub-flow split in User Flow (only if applicable)

- [ ] [Given this condition, this result happens]
- [ ] [Given this error, this handling happens]
```

**Basic flow (no sub-flows):**

```markdown
## 3-1. [Flow name]

[Policy/context notes — only if applicable]

### User Flow

1. [Action]
2. [System response]
3. [Result]
   - Success → [Result]
   - Failure → [Error handling]

### Acceptance Criteria

- [ ] [Given this condition, this result happens]
- [ ] [Given this error, this handling happens]
- [ ] [Given this edge case, this behavior happens]
```

**With sub-flows (create/edit/delete, bulk/single, etc.):**

```markdown
## 3-3. [Flow name]

### User Flow

**[Sub-flow A]**

1. [Action]
2. [Result]

**[Sub-flow B]**

1. [Action]
2. [Result]

### Acceptance Criteria

**[Sub-flow A]**

- [ ] [Condition]

**[Sub-flow B]**

- [ ] [Condition]
```

**Good AC examples:**
```
- [ ] Creation is blocked when the name field is empty
- [ ] An error message is shown and creation is blocked on a duplicate name
- [ ] A successful creation navigates to an empty detail screen
```

**Bad AC examples:**
```
- [ ] Collection gets created properly          ← too vague
- [ ] Calls the POST /collections API           ← technical implementation (engineering's territory)
- [ ] Gets saved to the DB                       ← technical implementation (engineering's territory)
- [ ] [A concept from a later flow] gets applied  ← violates the incremental principle
```

---

### Section 4. UI & Policy Definition

**Write UI composition and policy together, per feature.** This way readers don't have to bounce between two sections to understand the same feature.

- Figma is the source of truth for UI visual specs (layout, components, color values) — just link to it
- Consolidate error messages into a single subsection at the end

```markdown
# 4. UI & Policy Definition

See Figma for UI visual specs (layout, components, colors)
[Figma link]

---

## 4-1. [Feature/screen name]

**UI composition**

- [Screen element 1]
- [Screen element 2]

**[Related menu/component] (if applicable)**

| Item | Description |
|---|---|
| [Item] | [Description] |

**[Policy name]**

| Item | Policy |
|---|---|
| [Item] | [Policy content] |

---

## 4-2. [Feature/screen name]

...

---

## 4-N. Error messages

| Case | Message | Where it appears | Action |
|---|---|---|---|
| [Error condition] | [Message copy] | [Location] | [Undo/Close/etc.] |
| [Error condition] | TBD | TBD | TBD |
```

---

### Section 5. Phase Plan

Define this after the first spec meeting with engineering, based on technical dependencies and deployment units. Leave it as a placeholder before that meeting.

```markdown
# 5. Phase Plan

To be defined after the first spec meeting with engineering, based on technical dependencies and deployment units
```

Convert to a table after the spec meeting:

```markdown
# 5. Phase Plan

| Phase | Goal | Key deliverables | Linear milestone |
|---|---|---|---|
| Phase 1 | [Goal] | [Deliverable] | TBD |
| Phase 2 | [Goal] | [Deliverable] | TBD |
```

---

### Section 6. KPI

Define this using real data after release and pilot operation. Leave it as a placeholder before release.

```markdown
# 6. KPI

To be defined using real data after release and pilot operation
```

Convert to a table after release:

```markdown
# 6. KPI

| Key outcome | Metric | Target |
|---|---|---|
| [Outcome name] | [Measurable metric] | TBD |
```

---

### Section 7. Q&A

Record open questions or decisions that came up during discussion.

```markdown
# 7. Q&A

| | Question | Answer |
|---|---|---|
| 1 | [Question] | [Answer or TBD] |
| 2 | | |
```

---

## Checklist after finishing a draft

```
[ ] Document header includes Figma link and Linear link (TBD if none)
[ ] No H1 document title (goes in the Notion page title instead)
[ ] Section 1: one-sentence background summary + target table
[ ] Section 2: Outcome / Output / Not Doing all written
[ ] Section 3: flow and AC merged, error cases included
[ ] Section 4: Figma link + per-feature UI & policy + consolidated error messages
[ ] Section 5: Phase placeholder or table
[ ] Section 6: KPI placeholder or table
[ ] Section 7: Q&A included (even an empty table if there's nothing yet)
[ ] Confirm there are no ✅/🔲 markers
[ ] Confirm there are no pixel/dimension specs (replaced with a Figma link)
[ ] Confirm AC contains no technical implementation details (API, DB)
[ ] Confirm AC is consistently phrased as noun/verb form
```

---

## Linear issue structure guide

Map PRD flows (3-X) to Linear issues 1:1.

**Structure:**
- **Project** = the whole feature (e.g. Collection)
- **Issue** = one user-flow unit (3-1 through 3-N), matching the PRD numbering
- **Sub-issue** = used when splitting BE/FE responsibilities (engineering creates these autonomously)
- **Label** = `backend` / `frontend` / `policy` / `ui-change`

**Rules for sizing an issue:**
- Group tightly-coupled flows within the same screen into a single issue
- Separate interactions (actions) from new pages (screen composition) — they're different types of work
- Consider splitting out flows that involve BE integration into their own issue

**Issue description template:**
```
PRD reference: [3-X]
Figma: [link]

[1-2 line flow summary]

AC:
- [ ] ...
```
