---
name: delta-bug-report
description: |
  Review a git delta (baseline commit → latest commit) for client-side logic bugs
  and produce a structured bug report: one-line summary per bug in your team's language,
  plus English technical detail (file:line, root cause, repro, fix direction) ready to
  paste into an IDE coding agent.

  Use when:
  - "Check this delta for bugs" / "Review these changes" requests
  - "Make a bug report to hand off to engineers"
  - After pulling the latest code, before deploying to production
  - Post-release / post-migration sanity check on what broke
---

# Delta Bug Report Skill

## Core principles

- **Find bugs, not test cases.** The goal is to surface broken logic in the delta, not document correct behavior. If the code works as intended, say "clean" and move on — never manufacture findings.
- **Lock scope to client-side.** If the backend lives in a separate repo, tell each subagent: "assume the backend contract is correct; only find bugs in the frontend logic itself." Verifying the backend contract is a completely different (and far more expensive) job — don't do it unless the user explicitly asks.
- **Separate confirmed bugs from "needs confirmation."** Never assert an ambiguous case as a bug. Intentional UX decisions have been misreported as bugs before (e.g. a "show-and-immediately-edit" pattern that looked like a missing error state but was the designed flow). When in doubt, put it in the "needs confirmation" section — only things you're certain about go in "confirmed."
- **Check production vs. demo/prototype first.** Branch names ("demo-polish"), README, or project notes reveal maturity. In demo/prototype repos, controls that exist but have no effect on most screens are often intentional incomplete wiring, not logic bugs. Only call something a confirmed bug if the underlying logic is *wrong* (e.g. a value is converted but mislabeled), not merely *incomplete* (e.g. a toggle that simply isn't wired up anywhere).
- **Re-verify at least a few confirmed bugs before reporting.** Don't trust subagent reports blindly — for things checkable in one grep/read (missing condition, wrong operator, typo), open the file and confirm yourself before calling it confirmed.
- **Output is a markdown issue document, not a spreadsheet.** Each bug section is self-contained: file path, root cause, repro scenario, and fix direction — readable independently without needing to read the whole document first.
- **One-line human summary per bug, in your team's language.** Above the English technical detail, write one sentence that lets an engineer decide in five seconds whether to act on it — symptom + impact, not a translation of the technical detail. Example (good): "Magic link login: after a failed request, the retry button stays permanently disabled and the user must hard-refresh." (bad): "The disabled prop is bound to the error state."
- **Record the commit range in the document header.** This lets the next review start with `git log <this head>..<next head> --stat` and get an exact delta — no guessing.
- **Follow the project's convention for where to save docs.** Each project has a place where these accumulate (repo docs/, Notion, Obsidian, etc.) — check and use it rather than picking an arbitrary path.

## Workflow

0. **Check the model.** Verify the current model before starting. If it is a reasoning-heavy model (large o-series, extended thinking, etc.), flag it: code review works best with a fast, code-capable model — reasoning-heavy models read files slowly, burn tokens, and don't produce better findings for this task. Ask the user to switch before proceeding.

1. **Check project maturity.** Scan branch name, README, and any project notes to determine whether this is production code or a demo/prototype. Carry that determination into every subagent prompt so findings are calibrated correctly.

2. **Size up — always report before proceeding.**
   ```
   git fetch
   git log --oneline <base>..<head> | wc -l
   git diff --stat <base>..<head>
   ```
   Show the user the result (N commits, N files, +M/-K lines) and confirm they want to continue. The delta may be larger than expected; the user may want to narrow scope or defer.

3. **Solo vs. subagents.** Small delta (roughly ≤15 commits / ≤30 files): handle in the main session. Larger: propose a split plan — "N files across X subagents in parallel" — confirm with the user, then proceed to step 4.

4. **Group by feature area, assign one subagent per group.** Split changed files into logical groups (new feature A, new feature B, large refactor of existing area, miscellaneous small changes). See `references/subagent-prompt-template.md` for the prompt template — key elements: exact file path list, commit message / ticket context, specific hypotheses about suspicious areas, explicit client-only scope, a "skip if it's just copy changes" time-saving note, a 500-word response cap, and the confirmed / needs-confirmation split instruction.

5. **Run subagents foreground, not background.** Background agents can die silently and lose results. If a subagent dies mid-run, resume it via its returned `agentId` rather than spawning a new one — it won't re-read files it already processed.

6. **Verification pass.** Once all groups report back, pick 2–3 mechanically checkable confirmed bugs and verify them directly by opening the file.

7. **Write the document.** Use the format in `references/output-format-example.md` — severity order (High / Medium / Low / Needs confirmation). Save to wherever the project accumulates this kind of document.

## Reference

- **Use a service-hardening checklist as your bug class hints.** Common classes: replace-merge data loss, silent swallow of save failures, trusting client-derived values as server output, in-memory cache in serverless, missing AbortSignal propagation, re-running expensive steps on retry. Pick the classes relevant to this delta and give them to subagents as hypotheses to check.
- **Front-load known context into subagent prompts.** If a file was already reviewed in a prior delivery, or a component is shared across features, say so — don't make the subagent rediscover it from scratch.
- **File paths may have moved.** If a subagent can't find a path (refactor, rename), let it search for the correct location rather than failing silently.
