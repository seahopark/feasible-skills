---
name: policy-doc
description: |
  Structures, generates, and maintains service policy documents in Markdown.
  Use it to convert a PRD or a TC spreadsheet into a policy doc, or to update an existing one.
  When an issue tracker (Linear, Jira, GitHub Issues, etc.) is connected, it looks up related context first and links it in; when the service's code repo is accessible, it cross-checks the doc against the actual implementation to improve accuracy.

  Use this skill whenever:
  - Someone asks to write a policy doc ("make a policy doc", "write policy.md")
  - Someone asks to convert a PRD or analyze a TC sheet into documentation
  - Someone asks for a UX spec ("write a ux-spec", "put together the UX spec")
  - Someone asks to update common-ux.md or add a shared rule ("update common-ux", "add a common rule")
  - A section needs to be added to, edited in, or reviewed in an existing policy doc
  - A policy doc needs to be generated from an issue or a PRD
  - An existing policy doc needs to be verified/updated against the actual implementation (code)
---

# Policy Document Management Skill

## Core principles

- Keep a **consistent structure so planning, engineering, and QA can all reference the same source of truth**
- Mark the evidence status of every item explicitly using the "evidence-status notation" below (never leave it blank)
- When evidence spans multiple sources (code repo, issue tracker, existing docs), **never conclude from just one** — cross-check
- Don't write a changelog section (Git already handles that)
- Delegate visual details (colors/spacing/fonts) to Figma; don't put them in the document

---

## Evidence-status notation

It's common for planning docs and the actual implementation to diverge. Collapsing "not decided yet" and "confirmed to not exist" into the same notation erodes trust in the document and leads engineering/QA to work off wrong assumptions. Use these four levels instead.

| Notation | Meaning | When to use it |
|---|---|---|
| `[confirmed]` | Confirmed with clear evidence | Cite the source alongside it (code location, doc name, issue number, etc.) |
| `[confirmed: absent]` | Confirmed that the feature/field/constraint does not exist | For when the answer is "not part of the policy," not "undecided." Means you looked for the relevant logic and it isn't there — distinct from simply not having found it |
| `[needs confirmation]` | No evidence found, unconfirmed | Don't assume it's absent — leave it unconfirmed |
| `[contradiction]` | Sources disagree | Cite every conflicting source, and if possible note which one you judged more trustworthy (usually the actual implementation/code) and why |

---

## Step 0: Confirm context up front

Before writing or updating a document, check the three things below, in order, to the extent you can. Skip to the next step if something doesn't apply.

### 0-1. Issue-tracker context (if connected)

```
Priority order:
1. A URL was given directly → look it up with web_fetch or the relevant MCP tool
2. There's an issue number/ID → look it up via the connected issue-tracker MCP
3. Only a project/feature name exists → search the issue tracker
4. Nothing at all → proceed to 0-2
```

What to confirm: scope, current status (Planned/In Progress/Done), milestone/owner, links to any existing PRD or policy doc.

### 0-2. Search existing documents first

Check whether the workspace/document store already has a past PRD, planning draft, or meeting notes covering the same domain. Starting from a blank slate ("reverse-engineer everything") risks missing a more detailed document that already exists. If you find one, treat it as the baseline and only update it against the code/issue tracker to bring it current — that's more accurate and faster than writing from scratch.

### 0-3. Codebase verification (if a connected repo exists)

If you can access the source code for the service the policy doc covers, look for evidence in this priority order before writing or verifying. Planning docs and actual implementation frequently diverge, so let the code have the final say wherever possible.

1. **Decision records** (ADRs, decision logs, etc.) — the fastest place to find "why was this decided this way." Many questions that look like `[needs confirmation]` may already be answered here.
2. **Domain model** (enums, type/schema definitions) — pull state values and field names directly from here rather than guessing from the screen.
3. **Schema change history** (migration file names, related commit log) — skimming file names alone often reveals rename/removal/migration history.
4. **Test code** — test names and descriptions are often the actual behavioral contract. Especially useful for confirming edge cases.
5. **Machine-readable contract files** (OpenAPI specs, schema exports, route-ownership mappings, etc.) — when they exist, they're evidence you can always diff against current state.

**Cross-checking principle**: if a service is split across multiple repos/modules, a feature that looks like it disappeared from one repo might have simply moved to another. Don't conclude "this feature is gone" from a single repo — cross-check related repos.

**Distinguish what's actually stored**: when documenting a field or value, mark whether it's a value stored server-side or a derived value the client computes each time (see template section 6). Failing to distinguish this can make a client-computed value look like "something the server judges and returns," which distorts the policy's core claims (accuracy, trust level, etc.).

**Distinguish terminology**: the display name shown on screen (UI label) and the internal model name in code/DB sometimes differ (e.g. due to a rename history). Don't collapse them into one — record both (see template section 2).

---

## Step 1: Decide the document type

When a request comes in, first decide which document to produce using the criteria below.

| Request | Document to create | Reference |
|---|---|---|
| "clean up the ~ policy," "allow/deny rules" | `policy.md` | references/template-policy.md |
| "define screen behavior," "what happens on click" | `ux-spec.md` | references/template-ux-spec.md |
| "add a common rule," "a pattern that applies service-wide" | `common-ux.md` | references/template-common-ux.md |
| A TC spreadsheet is attached | Decide after analysis | references/tc-analysis-guide.md |
| Converting a PRD into a policy doc | policy.md + ux-spec.md | both |
| Verifying/updating an existing policy doc against code | target policy.md (not a new doc) | run Step 0-3 first |

---

## Step 2: Decide the file path

Lay out paths under `/docs/policy/` to match the service's domain structure. Example:

```
/docs/policy/
├── common-ux.md
├── {domain-A}/
│   ├── policy.md
│   ├── ux-spec.md
│   └── {sub-feature}.md
├── {domain-B}/
│   ├── policy.md
│   └── ux-spec.md
└── {cross-cutting concern, e.g. notifications/permissions}/
    └── policy.md
```

If a tree already exists, follow its structure first. If not, propose a new path organized by domain (the feature boundaries the product actually splits along) and confirm before proceeding.

---

## Step 3: When to split a document that's grown too large

When a single policy document keeps accumulating sub-domains with different owners, it gets harder to read and updates start getting missed. Split it into a hub document plus per-domain sub-documents when any of these apply.

**Signs it's time to split:**
- A single document mixes 3+ sub-domains with different ownership
- Finding content for a specific domain takes a long scroll/search
- The list of open questions is lumped together with no domain separation, so ownership is unclear

**How to split:**
- Hub document: keep only the overall overview, a module map (links to sub-documents), cross-module dependencies, and the information architecture
- Domain documents: keep only that domain's definitions, rules, open questions, and data fields
- Update links in any other documents that referenced the hub (story maps, roadmaps, etc.) to point at the sub-document that now holds the actual content

---

## Step 4: Deciding what's a common rule

Before writing content, decide whether each rule belongs in common-ux.md.

**Put it in common-ux.md when:**
- The same rule is used identically across 2+ domains
- It's a pattern that must apply consistently across the whole service
- It's a behavior/numeric rule, not a visual effect (time formatting, number formatting, etc.)

**How to note an exception in a domain document:**
```markdown
| Created date         | → [Common UX Definitions > Time Formatting]     |
| Created date, exception | Not shown after 24 hours (ranking screen only) |
```
Link to the common rule first, then note the exception directly below it.

---

## Step 5: Classifying policy vs. UX spec

When writing content, use these criteria to decide which document it belongs in.

| If the content reads like... | It goes in |
|---|---|
| "must do ~ / must not do ~" | policy.md |
| "when ~, show ~" (condition/state) | policy.md |
| "up to n lines / n items" (a number) | ux-spec.md |
| "clicking navigates to ~" (behavior) | ux-spec.md |
| "shown in ~ color / at ~ size" (visual) | Figma (don't put it in the doc) |
| "on hover, ~" (interaction) | common-ux.md |

---

## Step 6: Write the document

Load the relevant template and write. Each template lives in references/.

- Writing `policy.md` → load `references/template-policy.md`
- Writing `ux-spec.md` → load `references/template-ux-spec.md`
- Editing `common-ux.md` → load `references/template-common-ux.md`
- Analyzing a TC sheet → load `references/tc-analysis-guide.md`

---

## Step 7: Issue-tracker integration (optional)

After finishing the document, if there's a connected issue tracker and either a request or a related issue exists:

```
1. Register the policy doc in the issue tracker's document system
2. Add a "policy doc completed" comment to the related issue
3. If there are 3+ [needs confirmation] or [contradiction] items → propose creating a separate issue
```

In environments without an issue tracker, just compile the list of `[needs confirmation]`/`[contradiction]` items separately and hand it over.

---

## Appendix: things to watch for when applying this skill

The steps and thresholds in this skill (document-split thresholds, codebase-verification priority order, etc.) are generalized to work across many services — they're not a precise fit for every domain. Domain structure, which rules matter most, and what's verifiable varies a lot by service, so:

- The first time you apply this to a new domain/service, don't follow the steps mechanically — check with the document owner first about whether there's anything unusually important or unusual about this domain.
- If, while continuously updating documents, this skill's criteria (document-split signals, evidence-status notation, common-vs-domain boundary calls) stop matching reality, don't push through with your own judgment on the spot — confirm with the document owner, and update the skill itself if needed.
- In short, treat this skill as a living guide that needs continuous refinement through use, not something you set up once and leave alone.
