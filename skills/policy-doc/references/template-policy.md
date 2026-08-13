# Domain policy document template (policy.md)

Contains: what's allowed/forbidden/mandated (the rules)
Doesn't contain: visual effects, layout details, history

For evidence-status notation (`[confirmed]` / `[confirmed: absent]` / `[needs confirmation]` / `[contradiction]`), see "Evidence-status notation" in SKILL.md.

---

```markdown
# {Domain name} Domain Policy

> **Document type**: Policy reference
> **Audience**: Planning / Design / Engineering / QA
> **UX spec detail**: → [{domain}/ux-spec.md]
> **Common UX rules**: → [common-ux.md]

---

## 1. Overview

### 1.1 What this domain does
[needs confirmation], or a one-sentence role definition

### 1.2 Scope
| Category | Included |
|---|---|
| Screens | |
| Data | |
| Excluded | |

### 1.3 Related domains
- **{domain}** → {reason for the connection}

---

## 2. Core definitions

### 2.1 {State value / type definition}
| Internal model name | UI display name | Description | Notes |
|---|---|---|---|
| | | | |

If the UI display name and the internal model name (code/DB) are the same, one value is enough — but if they differ (e.g. due to a rename), record both. Don't guess the value name from on-screen copy alone — treat the actual value in code/schema as the source of truth.

---

## 3. Rules

### 3.1 {Rule name}

| Item | Rule |
|---|---|
| Condition | |
| Exception | |
| Client difference | Web / App / mWeb |
| Evidence | [confirmed: {source}] / [needs confirmation] |

### 3.2 {Rule name}
...

---

## 4. Process (only for domains with an operational flow)

[needs confirmation], or describe the flow/steps

---

## 5. UX references

| Item | Referenced document |
|---|---|
| UX spec | → [{domain}/ux-spec.md] |
| Common interactions | → [common-ux.md > Interaction patterns] |

---

## 6. Data field summary

| Group | Field | Storage owner | Description |
|---|---|---|---|
| | | Server-stored / Client-derived | |

Always distinguish "storage owner." If a value the client computes on the fly (e.g. a threshold-based rating) is written as if it were server-stored, the policy's core claims (accuracy, trust level, etc.) can end up misrepresented.

---

## 7. Open Questions

| # | Question | Status | Evidence/source |
|---|---|---|---|
| Q1 | | [needs confirmation] | |

- Once a number is assigned, never reuse it (when a question is resolved, update the status — don't delete the number).
- When status changes to `[confirmed]` or `[confirmed: absent]`, note what confirmed it in the evidence/source column (code location, doc name, issue number, etc.).
- When re-verifying this domain document against the codebase (SKILL.md Step 0-3), start with this table — check first whether any `[needs confirmation]` items have been resolved.
```
