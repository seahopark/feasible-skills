---
name: functional-tc-authoring
description: |
  Writes a manual-QA functional test-case (TC) spreadsheet grounded in the actual frontend code.
  Reads the code screen by screen / feature by feature to extract exact copy, conditions, and states, and records it in a standard 6-column format in a sheet (usually Google Sheets).
  This runs in the opposite direction from the tc-analysis-guide in the policy-doc skill — that one is "read an existing TC spreadsheet and write a policy doc,"
  while this one is "read the code and write a new TC."

  Use this skill when:
  - Someone asks you to write TCs, build a test-case sheet, or author functional test cases
  - Someone asks you to analyze the code for a screen/feature and turn it into TCs
  - A new screen/section needs to be added to an existing TC sheet
  - You need to verify or fill gaps in an existing TC sheet by cross-checking it against the code
---

# Functional TC-Authoring Skill

## Core principles

- **Code is the source of truth.** Don't guess from the UI alone — pull exact button copy, error messages, and conditions directly from the actual frontend source (component files) and quote them verbatim. Don't paraphrase — QA compares the sheet against on-screen copy character for character.
- **Pilot first.** Before applying this to the whole set of screens/features, hand-write one small feature end-to-end, including the code cross-check, and use that process to validate and adjust the column structure itself. Only expand to the rest once the format has settled.
- **Functional tests only.** If the user says "functional only," load/stress/concurrency testing is explicitly out of scope — don't silently include it, and don't silently drop it either. Call it out as "a separate workstream."
- **A bug you find isn't a TC.** While reading code, you'll run into real bugs, inconsistent behavior between similar flows, copy errors, or demo/mockup code with no real backend wired up. Don't quietly fold these into TC rows as if they were normal scenarios — see the "Issue triage" section below.
- **Check existing project knowledge first.** If there's already accumulated memory, spec docs, or prior code-audit notes for this product/codebase, skim them before writing TCs. Rather than burning time rediscovering the same issue from scratch, cross-reference it with what's already known and refine further — e.g., you might confirm in code that "this button is hidden in production," but a past audit might already have the sharper finding that "the hide condition references a server-only env var inside a client component, so it always evaluates to undefined."

## The 6-column format

| # | Test Case | Precondition | Action | Expected Feedback | Feedback Type |
|---|---|---|---|---|---|

- **#**: a short per-section prefix + a 2-digit number (`P-01`, `BA-03`). Coin the prefix on the fly from the section's topic (list → `L-`, bulk action → `BA-`, row action → `RA-`, etc.); it just needs to be unique within the document.
- **Expected Feedback**: quote the exact copy confirmed in code, in double quotes. If you're not sure, don't hedge in the cell — append "(needs confirmation)" to the Feedback Type column instead.
- **Feedback Type**: reuse the emoji categories defined in the Legends tab (🔴 inline error, 📢 toolbar/toast notification, ⚠️ form error banner, 🚫 button disabled, ➡️ page navigation, etc.); fall back to free text (`screen state change`, `modal opens`/`modal closes`, `list refresh`) when nothing fits. Define the emoji set once per project and reuse it from then on — see `references/sheet-format-example.md` for concrete examples.

## Document/sheet structure

- **One Legends tab**: defines the feedback-type emoji vocabulary in a single place, reused across the whole document.
- **One tab per feature area**: e.g. "Projects", "Library". Don't cram everything into one tab — start a new tab when the feature area changes.
- **Numbered sections within a tab**: `title row` → `one-line description row (which UI area/component this covers)` → `6-column header row` → `data rows` → one blank row before the next section. See `references/sheet-format-example.md` for a worked example.

## The code-grounding workflow

For each feature area, confirm the following directly in code (don't fill these in from looking at the UI alone):

- Per-field validation rules (required / maxLength / format) and the **exact** error message copy
- Button-disabled conditions
- Loading/pending state — what text or spinner it switches to, and whether other buttons lock at the same time
- Behavior on API success (redirect? toast copy? modal closes? does the list refresh via a cache patch or a full refetch?)
- Behavior on API failure (inline text? banner? native `alert`? silently ignored?)
- Exact copy for empty states, toasts, and notifications
- **Whether publish/subscribe event pairs actually connect.** If you find a `notify*`/`dispatch*` call, or a sessionStorage/localStorage flag meant to "show something later," grep beyond the files you're currently reading to confirm a listener/subscriber actually reads it. A publish with no subscriber is a silent bug real users will never trigger a report for — this exact pattern (a sessionStorage flag written but never read, so a success toast never fires) was caught this way in a past session.

## Split large areas across subagents

A single feature area often crosses 1,000 lines of code — don't try to read it all in the main context. Split it logically into 2-3 groups and delegate them to Explore subagents in parallel (foreground). See `references/subagent-prompt-template.md` for the prompt template. The essentials:

- Explain the product/feature context and give exact file paths + line counts
- State explicitly: "report structured facts only, don't write TC rows yourself"
- Give a checklist of behavior categories to look for (filter/sort/search/select/bulk-action/pagination/row-action/empty-state/create-entry-point, etc., adjusted to the feature)
- Have it grep whether existing automated tests (`*.test.ts`/`*.spec.ts`) already cover this area, and report with evidence — a file **existing** and a file **substantively covering** the area are different things. Have it check whether tests are disabled by default via an env flag, or are shallow tests that verify some other dimension (e.g. a field's presence in an API response) rather than actual UI behavior.

Once subagent results come back, **converting them into TC rows is always done by the main agent** — don't delegate format/ID-scheme consistency to a subagent.

**Feed already-confirmed background facts into the prompt up front.** If a prior investigation already established something like "this repo has zero automated tests targeting this area" or "this feature is 100% local mockup with no API calls," don't make the subagent re-grep from scratch every time — phrase it in the prompt as "already-confirmed fact — verify/supplement only." Don't omit it entirely either; give it as a starting point and have the subagent add only newly discovered facts on top. This shortens responses and cuts down on redundant exploration.

**Use a separate template for re-verification (diff-based) investigation.** First-pass authoring and re-verification need different amounts of investigation — reusing the first-pass template for re-verification means re-sweeping unchanged areas at full length every time. See the "Re-verification mode" section in `references/subagent-prompt-template.md`.

## Verification pass

After writing a batch to the sheet, if the user provides a screenshot or confirms the live screen, cross-check it immediately and fix the sheet on the spot. Example: if a maxLength you inferred from code turns out different in the screenshot, correct it immediately — don't defer it.

## Coverage map — preparing for the next version/release

A TC sheet is a snapshot of "how the code at this commit behaves right now." If a major version bump is planned (a major release, a backend migration, etc.), treat the TC you just wrote as a "draft as of this point," not a permanent artifact, and prepare now so it can be re-verified efficiently later:

- **Record which code paths back each tab/section separately** (in a spreadsheet, a dedicated "Coverage Map" tab works — tab name | repo | baseline commit | covered paths (globs/file list) | date confirmed). TC rows themselves shouldn't contain file:line references (this is a QA-facing doc), but this mapping needs to live somewhere so you can quickly scope "what changed" next time.
- **Flag it explicitly when multiple tabs share the same file** (e.g. a single sidebar component serves as the entry point for two features) — if that file changes later, every related tab needs re-verification together.
- **When a new version ships**: don't rewrite everything. First check `git log <baseline-commit>..<new-tag/commit> --stat -- <covered-paths>` to see which tabs were touched → re-read the relevant files only for affected tabs and diff against the existing TC rows (update rows where copy/conditions changed, add rows for new behavior, remove or mark "removed in newer version" for behavior that's gone) → leave unaffected tabs untouched. After re-verifying, update the baseline commit/confirmation date in the coverage map.

## Issue triage

How to handle things you stumble into while reading code for TC authoring:

1. **A real bug / inconsistency / copy error** → don't quietly fold it into a TC row; report it to the user as a separate text list at the end of the turn. Don't write it directly into the sheet.
2. **Something that's both a testable behavior and an inconsistency issue** (e.g. "deleting via path A stays in place, deleting via path B navigates to the list") → write it as a normal TC row, but flag it with "(needs confirmation)" in the feedback type or feedback cell.
3. **An entire sub-flow turns out to be demo/mockup code with no backend wired up** (a code comment like "No API calls are made," for example) → leave a warning in that section's description row so nobody mistakes "it doesn't save" for a bug and reports it.

## Notes on the Google Sheets MCP tools

- Default to `sheets_get_values` (with an explicit range) for reads. `sheets_get_full_sheet_snapshot` includes formatting and is heavy — it fails on large sheets (thousands of rows) by exceeding the token limit.
- `sheets_update_values` handles both editing existing cells and bulk appends. Writing beyond the sheet's grid rowCount/columnCount throws an "exceeds grid limits" error — either grow the grid first with `sheets_update_sheet_properties`, or, for a new feature area, create a fresh tab with `sheets_insert_sheet` sized with enough rowCount/columnCount from the start.
- **This grid-limit issue isn't unique to newly created tabs.** When a human reviews the sheet and manually cleans up empty rows/columns, the existing tab's grid size shrinks along with it. Get in the habit of checking current rowCount/columnCount with `sheets_get_metadata` before appending a large batch to an existing tab, too.
- **When putting non-ASCII text (e.g. Korean) into the `values` array, don't hand-encode it as `\uXXXX` Unicode escapes.** Just write the plain UTF-8 text as-is — escaped form burns more tokens, and later attempts to match the same string with another tool (like Edit) will fail because the escaped form and the plain form don't match.
