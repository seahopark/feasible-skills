---
name: product-spec
description: |
  Reverse-engineers a SaaS product from screenshots, interviews, and hands-on usage, and structures the findings into two documents: Business Discovery (why we're building it) and Product Spec (what we're building).
  Use it when you're new to a team and need to understand a product's structure, when an existing product's feature definitions are missing or scattered, or when you need to turn screenshots/Figma/a live service into a spec document.
---

# Skill: Product Definition & Spec Documentation

Reverse-engineers a SaaS product from screenshots, interviews, and hands-on usage, and structures the findings into a Business Discovery document and a Product Spec document.

---

## When to use it

- When you've just joined a team and need to understand a product's structure for the first time
- When an existing product has no feature definitions, or they're scattered across places
- When you need to turn screenshots, Figma files, or a live service into a spec document
- When engineering or design needs a feature definition doc handed to them

---

## The two-document principle

Always split the output into two documents: **Business Discovery** and **Product Spec**.

| Document | What it contains | Audience |
|------|---------|------|
| **Business Discovery** | Why we're building it — pain points, hypotheses, market, impact | Founders, investors, sales |
| **Product Spec** | What we're building — modules, entities, behavior, open points | Engineering, design, PM |

Don't mix them into one document. "Why" and "what" have different audiences and different update cadences.

---

## Reverse-engineering from screenshots

A screenshot isn't just something to "describe." The goal is to pull the **service layers** apart from what's on screen.

### The service-layer separation concept

A single screenshot mixes three layers together.

| Layer | Meaning | What it becomes in the spec |
|--------|------|----------------|
| **Data layer** | The units of information shown on screen | Entity field definitions |
| **State layer** | The states data can be in and the conditions that transition between them | State machine definitions |
| **Action layer** | What the user can do and what happens as a result | Flow definitions |

UI layout (where things are placed on screen) belongs to **design**, not the spec — leave it out.

---

### Analysis methods by screenshot type

#### List screens (table/list views)

```
What you see          → What you extract
─────────────────────────────────────
Column headers         → Entity field names
Row values              → Field type / example values
Status badges           → List of Status values
Icon types              → Enum type (e.g. file format)
Filter tabs              → Classification criteria / taxonomy
Sort order              → Default sort field
Row-level "..." menu     → List of possible actions
```

**Example — a Projects list screen:**
- "Due", "Project", "Status", "Client", "Assignees" columns → Project entity fields
- Word/Excel icons → `source_file.file_type` = `"docx"` | `"xlsx"`
- "In Progress" / "Not Started" badges → Project Status values
- "Project / Process / Assignee" tabs → filter classification criteria (definitions of each condition become an open point)

#### Detail/edit screens (forms, modals)

```
What you see          → What you extract
─────────────────────────────────────
Required marker (*)     → required field
Input field type          → type (text / date / textarea)
Placeholder text          → hint about the field's purpose
Disabled fields            → conditional edit constraints
Save/Cancel buttons        → transaction boundary
```

**Example — a Project Setting modal:**
- `*` on Title → required field
- Due Date formatted as "31/07/2026" → DD/MM/YYYY format
- Description placeholder "Provides context to help members understand..." → makes the field's purpose explicit

#### Status dropdowns and badges

```
What you see          → What you extract
─────────────────────────────────────
Full dropdown item list    → complete list of Status values
Icons / colors               → visual meaning per status
Currently selected value     → inferred default value
Item order                   → inferred direction of state transitions
```

**Example — a Question Status dropdown:**
- "Not Started / In Progress / In Review / Approved" → a 4-stage state machine
- gray empty circle / orange circle / blue spinner / green check → inferable meaning per state
- order flows Not Started → Approved → inferred as a one-way transition (whether rejection allows moving backward is an open point)

#### Side panels / slide-ins

```
What you see          → What you extract
─────────────────────────────────────
Panel title             → the entity it's tied to
Info structure inside     → nested entity fields
"See file >" link          → cross-module relationships
Badges / tags              → classification value types
```

**Example — a Sources side panel:**
- file name / folder path / update timestamp / High badge / full answer text / "See file >" → the complete set of `source_item` entity fields

#### Banners and notification areas

```
What you see          → What you extract
─────────────────────────────────────
Banner copy             → a value the system auto-computes
Number formatting          → inferred aggregation logic
Conditions for the banner to appear → the triggering event
```

**Example — an "AI Matched 83% of Questions" banner:**
- "248 matches - 180 Exact · 27 High Confidence" → the structure of the match-result aggregate
- 83% = matched questions / total questions → the aggregation logic
- banner placement (top of Overview) → auto-computed on project entry

---

### Order of analysis when you receive a set of screenshots

When you're handed multiple screenshots, work through them in this order.

**1. Navigation first** — Look at the sidebar or tab structure first to map out the list of modules and their hierarchy.

**2. List screens, then detail screens** — List screens show entity fields in the most compressed form. Detail screens fill in additional fields the list didn't show.

**3. Collect every state value** — Gather all state values from dropdowns, badges, and filter items, then draw the state machine.

**4. Find cross-module connections** — Trace data flow between modules through things like "See file >" links, source citations, and linked file names.

**5. Write down what you can't see** — Anything not confirmed in the screenshots stays an open point. Don't fill gaps with guesses.

---

### A note on test data

When reverse-engineering, what's on screen might be test data.

| Situation to be suspicious of | How to handle it |
|---------------|----------|
| Numbers are unusually clean or round | Note "based on test data"; update with real numbers once confirmed |
| Data looks unrelated to real operations | Annotate as "test environment" until confirmed by the owner |
| A feature on screen doesn't actually work | Note "in development — not yet functional" |
| Inconsistent data | Don't assume bug vs. intentional design until confirmed |

---

## The full reverse-engineering process

```
[Collect screenshots]
  User shares screenshots or you access the live service directly
        ↓
[Step 1: Map out the module list]
  Navigation structure → module names + one-line role definitions
        ↓
[Step 2: Screenshots → layer separation]
  Data layer   → extract entities and fields
  State layer  → extract state values and transition direction
  Action layer → extract user actions and flows
        ↓
[Step 3: Write the draft]
  Apply the module template (role → entities → actions → open points)
        ↓
[Step 4: Gap analysis]
  Check for uncovered areas per module
  Confirm data flow connections between modules
  Identify commonly undefined areas (permissions, notifications, onboarding, etc.)
        ↓
[Step 5: User verification and correction]
  Share the draft with the actual owner → fix what's wrong
  Confirm which parts are test data vs. real data
        ↓
[Repeat]
  New screenshots arrive → update only the affected section
```

---

## Business Discovery structure

```
# Business Discovery

## Summary
The core thesis in 2-3 lines

## Background
Market context / customer's current workflow (as-is) / key numbers

## Pain points
The manual-process flow, plus a table of real business risk

## Customers & journey
Personas (Primary / Secondary / Gatekeeper)
As-is journey → to-be journey, plus the flywheel

## Strategic approach
Target segment / competitors / positioning / core value proposition

## Hypotheses
| # | Hypothesis | Validation method | Status |
Separate core hypotheses from supporting ones

## Impact
Quantitative (metrics to measure) + qualitative (change in user experience)
```

**Writing tips:**
- Every hypothesis needs a "validation method" and a "status" (unvalidated / validating / confirmed) attached
- Draw pain points as a step-by-step flow of the actual workflow, and call out the pain at each step
- If you don't have impact numbers, write "needs measurement" — don't assume a number

---

## Product Spec structure

### Module template (apply identically to every module)

```
# [Module name]
> One-line role description

## Entities
(data structure)

## Actions
(only for modules that need it — skip for simple modules)

## Open points
- [ ] Unconfirmed item
```

### Entity-writing rules

Always use a code block. One block per entity.

```
EntityName
├── field_name    description / possible values
├── field_name    description
└── nested[]
    └── sub_field    description
```

- Mark required fields with `*` or `(required)`
- Mark unconfirmed fields inline with `→ needs confirmation`
- Don't describe UI panel shapes or screen layout — that's not spec content

### Action-writing rules

- If there's a state transition, write the state table together with the transition flow
- Represent flows as an arrow diagram inside a code block
- Always state "who does this" (author / approver / system)

### Open-point rules

- Keep per-module open points only at the end of that module's section — don't manage them in two places
- Split product-wide undefined areas into a `Common undefined areas` section at the bottom
- Open points are only for "things that need confirmation" — don't mix in guesses or suggestions

---

## Gap-analysis checklist

Once the spec draft is done, check its coverage against this list.

**Per module**
- [ ] Does every module have a role definition?
- [ ] Are each module's entities defined (and is the reason stated for modules with no data)?
- [ ] Is the state machine drawn for entities that have state transitions?
- [ ] Are per-module open points written as concrete questions?

**Across modules**
- [ ] Is cross-module data flow organized as a dependency diagram?
- [ ] If there's a flywheel (a feedback loop), is it clear where data feeds back in?
- [ ] Is it clear how data created in one module gets consumed by another?

**Common areas**
- [ ] Permissions/roles defined, or explicitly marked undefined
- [ ] Notification events defined, or explicitly marked undefined
- [ ] Onboarding flow defined, or explicitly marked undefined
- [ ] AI/data-processing approach defined, or explicitly marked undefined

---

## Structuring checklist

**Business Discovery**
- [ ] Is the core thesis organized around both "removing a problem" and "creating value"?
- [ ] Are pain points shown as a flow diagram rather than a flat list?
- [ ] Does every hypothesis have a validation method attached?
- [ ] Is it clear whether impact numbers are actual measurements or assumptions?

**Product Spec**
- [ ] Does every module follow the same 4-section template?
- [ ] Are all entities written as code blocks consistently?
- [ ] Is UI description (panel layout, etc.) kept out of the spec?
- [ ] Are open points free of duplication between the module-level and global sections?
- [ ] Is "needs confirmation" distinguished from "needs a product decision"?

---

## Related files
- User story mapping skill: `skills/user-story-mapping/SKILL.md`
