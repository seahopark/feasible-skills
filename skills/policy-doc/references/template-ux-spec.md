# UX spec document template (ux-spec.md)

Contains: how the screen actually behaves (implementation-level)
Doesn't contain:
- Visual effects like color/spacing/font → managed in Figma
- Interaction patterns like hover → managed in common-ux.md

For evidence-status notation (`[confirmed]` / `[confirmed: absent]` / `[needs confirmation]` / `[contradiction]`), see "Evidence-status notation" in SKILL.md — applies the same way here.

---

```markdown
# {Domain name} UX Spec

> **Document type**: UX implementation reference
> **Audience**: Engineering / QA
> **Visual effects / layout**: → see Figma
> **Interaction patterns**: → [common-ux.md > Interaction patterns]
> **Policy reference**: → [policy.md]
> **Common UX rules**: → [common-ux.md]

---

## 1. {Screen name}

### 1.1 {Component name}

| Item | Rule |
|---|---|
| Title line count | Max n lines, truncated with ellipsis |
| Click behavior | {navigation destination} |
| Client exception | Web / App / mWeb difference |
| Empty state | Hidden or default image |
| Logged out | → [common-ux.md > Logged-in/out branching] |

### 1.2 {Component name}
...

---

## 2. {Screen name}

### 2.1 {Component name}
...

---

## Client branching criteria
→ [common-ux.md > Client branching criteria]

| Item | Web | App | mWeb |
|---|---|---|---|
| | | | |
```

## Notes for writing this document

- Always specify numbers (max n lines, n items). Mark `[needs confirmation]` if you can't find evidence, and `[confirmed: absent]` if code/implementation confirms the constraint simply doesn't exist (don't collapse the two into one)
- Organize per-client differences into a table
- Reference common rules via a link; only note exceptions in this document
- Always define the empty state
