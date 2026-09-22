# Subagent Prompt Template

Elements to include in every subagent prompt when splitting a large delta across parallel agents. Template validated on a 32-commit / 121-file delta split into 5 groups — all groups returned usable findings.

```
Repo: <absolute path> (<stack description>, git already pulled to HEAD <head-commit> on branch <branch>).
Baseline commit for this review: <base-commit>.
Run `git diff <base>..<head> -- <paths>` for the paths below and read the full current file content where the diff context isn't enough (use `git show <head>:<path>` or read the file directly).

This is a client-side-only code review — do NOT reason about or flag anything related to whether the backend (<backend repo name>, a separate <language> repo) actually supports these endpoints/fields. Assume the backend contract is correct; only look for bugs in the frontend <language/framework> logic itself: broken conditionals, race conditions, missing null/error handling, incorrect state updates, stale closures, useEffect/useCallback dependency bugs, off-by-one errors, disabled UI that shouldn't be, etc.

<Maturity context: state whether this is production code or a demo/prototype.
For demo/prototype: "This is an early-stage demo — a control that exists but has no effect on most screens is often intentional (only the demo-script path was wired up), not a bug. Only report it as a confirmed bug if the underlying logic is actually WRONG (e.g. a value is converted/computed but mislabeled or mismatched), not merely INCOMPLETE (e.g. a toggle that simply isn't wired up yet). When in doubt, put it in 'needs confirmation', not 'confirmed bugs'."
For production: state that these are live features and should be treated as such.>

Context: <What this delta/group is for — ticket number, commit message summary, whether it's a new feature or a refactor>. <Specific hypotheses about suspicious areas — e.g. "Check whether the new filter interacts with the existing restore/rename/delete actions in a way that leaves stale references.">

Files/paths to review:
- <file1> (<line count changed> — one line on why it matters)
- <file2> ...
(For files that look like copy-only changes: "Scan the diff quickly; if there's no logic change, skip. If there is, review deeply.")

Report back concisely (under 500 words): for each confirmed bug, give file:line, a one-sentence description, and the concrete failure scenario (what input/state triggers it). Separate "confirmed bugs" from "looks unusual but might be intentional — needs product/engineering confirmation" (don't assert those as bugs, just flag them). If a sub-area is clean, say so briefly rather than padding the report. Do not modify any files — this is read-only analysis.
```

## How to group files

- **New features get their own group.** Highest risk — dedicate one subagent to each new feature, with its related files.
- **Large rewrites of an existing file (200+ line diff) are their own group candidate.** Bundle the 2–3 closely related files alongside it.
- **Pure copy/content-update commits go in one group** with the "scan and skip if no logic change" instruction. Exception: if a file in that group has an unusually large diff (100+ lines for what's billed as copy), flag it for deep review — pure copy changes shouldn't be that large.
- **Aim for ~5 groups.** More than that gets unwieldy to aggregate; fewer means each group is too large and the subagent will skim.

## Resuming a dead subagent

If a subagent dies mid-run (API error, spending limit, etc.), its partial progress is preserved under the returned `agentId`. Don't spawn a new agent. Send a message to that `agentId`: "Resume where you left off and give me the final report now." It won't re-read files it already processed, saving tokens.
