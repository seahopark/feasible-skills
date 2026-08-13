# Subagent delegation prompt template

For investigating a large feature area (roughly 1,000+ lines of code, 5+ files), fill in this template and send it to the `Agent` tool (subagent_type: Explore, run_in_background: false). Run it foreground since you need the result immediately. Split the area into 2-3 non-overlapping logical groups and send them in parallel in a single message.

```
Read these files in the repo at <path> (<one-line product/feature description>):

1. <file path> (<line count> lines)
2. <file path> (<line count> lines)
...
(if a file is 500+ lines: "skim structure first, then focus on interactive/state logic")

I am building a manual QA functional test-case (TC) spreadsheet — ONLY hand-testable
UI behaviors, explicitly EXCLUDING performance/load/stress/concurrency testing
(separate future workstream, out of scope here). I need STRUCTURED FACTS I can turn
into TC rows myself — do not write TC rows yourself, just report facts.

For each distinct interactive behavior you find, report:
- A short name for the scenario
- Precondition
- The user action that triggers it
- The exact observable feedback (exact button/label text, exact message text,
  disabled states, loading states, toasts, redirects)
- File:line reference

Specifically look for and report on:
- <checklist tailored to this feature, e.g. filtering/sorting/search/selection/
  bulk-actions/pagination/row-actions/empty-states/create-entry-point/drag-and-drop/
  view-toggle, etc.>

Also check: does any of these files have direct or nearby automated test coverage
(*.test.ts / *.spec.ts importing them, or e2e specs mentioning this area)? Report
yes/no per file with basis (import path match, not just filename guess).

Report as a structured list grouped by feature area. Keep it factual and compact —
under <1600-1800> words. This is read-only analysis, do not modify any files.
```

## How to choose the checklist items

What to ask about differs by feature type. For lists/tables:

- Sort (which columns are sortable? default sort?)
- Filter (how many dimensions? multi-select? how do you clear it?)
- Search (debounced? which fields match? empty-result copy?)
- Selection (single/select-all, scoped to the page or the whole set?)
- Bulk actions (anything beyond delete, confirmation flow, loading/success/failure feedback)
- Pagination/infinite scroll
- Row-level actions (hover menu, disabled conditions)
- Empty states (truly empty vs. zero results from a filter)
- Create entry point (does the button open a modal or navigate to a page?)

For forms/modals: per-field validation, error copy, button-disabled conditions, loading state, behavior on success/failure.

For approval/workflow features: the list of state transitions, which actions are available in each state, whether things can be undone/canceled, lock conditions (which states block editing).

## Re-verification mode (diff-based re-investigation after a version bump like 0.3.0)

The first-pass template above assumes "investigate this whole area from scratch and report." That assumption doesn't hold once the coverage map has confirmed a specific tab's covered paths changed and you're re-investigating — re-reporting unchanged parts at full length (1,600-1,800 words) every time is wasteful. Instead:

1. **Before** calling the subagent, the main agent reads the existing TC rows for that tab via `sheets_get_values`.
2. Narrow down to the actually-changed files with `git log <baseline-commit>..<new-commit> --stat -- <covered-paths>`.
3. Build the prompt from the template below — the key difference is instructing it to "report only the delta against known facts," with a much lower response cap:

```
Read these changed files in the repo at <path> (<one-line product/feature description>). These files
changed between commit <baseline-commit> and <new-commit> — a prior TC audit already covered this
area as of <baseline-commit>, so I only need what's DIFFERENT now.

Changed files:
1. <file path>
2. <file path>
...

Here is the existing TC sheet content for this area (from the prior audit) as ground
truth — treat every row as still true unless the current code contradicts it:

<paste existing TC rows in condensed table form — just the # / Test Case / Expected
Feedback columns is enough>

For each existing row, report ONLY if something changed:
- "STILL TRUE" rows do not need to be listed individually — just confirm the batch
  is unaffected if nothing changed in the files that back it
- "CHANGED" — old expected feedback vs new exact text/condition, file:line
- "REMOVED" — behavior no longer exists in the code
- "NEW" — a new interactive behavior not covered by any existing row (report with the
  same structure as first-pass authoring: scenario/precondition/action/exact feedback)

Also re-confirm automated test coverage status only if it plausibly changed (new
test files added near these paths) — don't re-grep the whole repo if nothing suggests
it would differ from the prior finding.

Keep it factual and compact — under 500 words unless the diff is unusually large.
This is read-only analysis, do not modify any files.
```

With this approach, unchanged tabs skip calling a subagent entirely, and changed tabs only get a "confirm the delta" pass instead of a full re-investigation — cutting token usage substantially.
