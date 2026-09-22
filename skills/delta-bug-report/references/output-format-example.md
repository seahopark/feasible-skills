# Output Format Example

Abbreviated template based on the structure of a real report (32-commit / 121-file delta review, 5 subagent groups, all findings independently verified).

```markdown
# <repo-name> Frontend Delta Bug Report

**Repo:** `<org>/<repo>` (branch `<branch>`)
**Commit range reviewed:** `<base>` → `<head>` (<date1> → <date2>, N commits, N files)
**Review scope:** Client-side (frontend) logic only. Backend contract correctness was explicitly out of scope — treat backend interactions as assumed-correct unless noted otherwise.
**Method:** git diff read against each changed file, cross-checked with current file state; N findings independently re-verified by direct code inspection (marked ✅ verified).

Each item below is self-contained — file path, root cause, and a concrete repro — so it can be picked up individually without reading this whole document end to end.

---

## High severity

### 1. <one-line title — symptom-first, not cause-first>
**Summary:** <One sentence in your team's language — just enough for an engineer to decide in 5 seconds whether to act. Symptom + user impact, not a translation of the technical detail.>
**File:** `<path>:<line>`

<Root cause in 1–3 sentences. Why this is a bug, contrasted against the commit intent or expected behavior.>

**Repro:** <Concrete scenario — what input or state triggers the failure>

**Suggested fix direction:** <One or two sentences on the fix approach — not necessarily full code>

---

(Repeat High → Medium → Low)

---

## Needs product/engineering confirmation (not asserted as bugs)

- **`<path>:<line>`** — <one-line summary in team language> / <English: what's ambiguous and why it wasn't called a confirmed bug>
- ...

---

## Explicitly out of scope for this report
<Things intentionally not reviewed — e.g. backend contract verification, a specific subsystem>
```

## Style rules

- **Titles describe the failure, not the file.** Bad: "LoginRequestScreen.tsx bug". Good: "Magic link login: retry button stays permanently disabled after a failed request."
- **Code snippets only when the eye needs to see it** (e.g. a missing space, a wrong operator) — never paste a whole function.
- **Severity thresholds:** High = core flow breaks or data integrity is compromised. Medium = visible misbehavior but user has a workaround, or it affects a secondary area. Low = cosmetic (wrong icon, typo, no functional impact).
- **"Needs confirmation" items still need a file:line reference** — so the team can find them without re-investigating.
- **The one-line summary is not a translation.** "What does the user experience, and why does it matter?" — not "The disabled prop is bound to the error state." The latter belongs in the technical detail section.
