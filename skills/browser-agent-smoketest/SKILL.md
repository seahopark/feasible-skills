---
name: browser-agent-smoketest
description: |
  Writes a full-cycle smoke test — a quick pass to confirm the whole service basically works — as a prompt
  that an AI browser agent (built for the Claude in Chrome side-panel extension) can read and execute on its
  own. Unlike functional-tc-authoring's TC sheet, which a human reads and clicks through by hand, the executor
  here is an AI, so execution meta-rules (result logging format, wait policy, file-upload technique,
  side-effect guards, cleanup) make up half the deliverable.

  Use this skill when:
  - Someone asks for a smoke test to check whether a whole service is basically working
  - Someone asks for a test script/prompt to run via a Claude-in-browser (Chrome extension) session
  - You need a light pass right after a new deploy or a big release to confirm core flows aren't broken
  - You already have a smoke-test spec and got back real run results (pass/fail log) to fold into the next version
---

# Browser-Agent Smoke Test Skill

## Core principles

- **The deliverable is a spec, not an execution.** This skill's job ends at producing a prompt you can paste straight into Claude in Chrome. Actually running it is a separate product/session's job — this skill never drives a browser itself.
- **Understanding the service is not this skill's job.** Reading the target service's code/spec to build an entity graph and spot recent drift reuses whatever method `functional-tc-authoring` or `delta-bug-report` already has (size up the repo → decide solo vs. sub-agent). This skill is a downstream step that takes that output (entity list, screen structure, recent changes) as an input assumption — it doesn't re-implement "read the code from scratch" internally.
- **The target executor is assumed to be Claude in Chrome (side-panel extension).** That extension has a dedicated `file_upload` tool that can target a page's `input[type=file]` element directly and inject a local/attached file. **Never make the agent click an upload button** — clicking triggers a native OS file picker, which an agent that can only see inside the browser can't see, so it just gets stuck there. For React-controlled inputs, setting the value alone may not register, so the spec must also pin down dispatching `input`+`change` events with `bubbles:true`. If you're targeting a different browser-automation tool instead, this whole section needs re-validating.
- **Fill the deliverable in incrementally.** The first version only needs to lightly cover execution rules, naming, test order, and known-normal behaviors. A UI anchor glossary (exact button labels, menu locations, DOM structure) and a "known issues already confirmed" list are information you can only get from at least one real run — forcing them in from the start just mixes in guesses. This isn't an accident; it's a structural property of information that requires measurement.
- **When run results come back, classify the cause.** For each failure, first split it into "a real app bug" vs. "a limitation of the browser agent/tool itself" (e.g., a field doesn't respond to Ctrl+A, an input has animated placeholder text that gets typed into literally). Keep the former as a bug. Absorb the latter into the next version's execution rules or a "known issue, don't re-report" section, so the same non-bug doesn't get reported over and over — this is the core mechanism behind v1 → v2 revisions.
- **Always include a cleanup step.** Every piece of test data you create gets a timestamp suffix, and cleanup wipes out everything under that prefix in one shot. That's a self-healing structure: even if cleanup fails this run, the next run sweeps it all up anyway, removing the single point of failure where "cleanup failing blocks the next run." That said, verify in the service-understanding step (e.g., grep for "already exists"-style error strings) which name fields actually enforce uniqueness, and note that precisely in the spec — not every field needs a timestamp, what matters is knowing which ones actually block you.
- **Contain side effects.** Pin approvers/assignees/mention targets to a single fixed test account before the run ("don't touch other members"), and always include "never modify or delete existing data — only what this run created" in the spec. Actions that are annoying to undo (export, real API calls, approval requests) are still fine to run for real as long as the target is isolated (a test account, data created in this run). Separately from deleting created data: if any step has to change a **shared setting** (workspace options, default filters, notification preferences — state this run didn't create), record the original value in the results table before changing it and pair it with a restore step in cleanup. Prefix-based bulk delete only removes what you *made*; it can't undo what you *changed*.
- **Verdicts are binary, but "couldn't observe" gets its own mark.** Alongside pass/fail, allow NT (Not Tested). If steps you never got to attempt — because an earlier step blocked them — are logged as failures, the real defect count inflates and the "when in doubt, fail" rule loses its meaning. Attempted but ambiguous → fail. Never attempted → NT. That's the only distinction.
- **Require evidence per verdict.** Give the results table an evidence column, and require failed/NT rows to carry an observed string (error message, toast text, status label) or a screenshot. When classifying "real bug vs. tool limitation" for the v1 → v2 revision, missing evidence means you have to reproduce first, which costs a full extra cycle.
- **Don't force a storage location for the deliverable.** If the project already has a documentation convention (Obsidian, in-repo docs, etc.), tell the user to follow that; otherwise, plain chat output is enough. Where to store it is left to the judgment of whoever's running the session. If it does go into a git-managed repo, keep one file and let commit history carry the versions — no date-suffixed copies.
- **A merge is evidence of implementation, not deployment.** Branch merges and code deltas found during service understanding only tell you "this feature exists in the code." Writing "this feature is active" into the spec needs separate evidence — release notes, runtime observation. Without it, write the step as conditional (run if the feature is visible, otherwise NT).

## Workflow

0. **Establish service understanding.** Run `functional-tc-authoring`'s or `delta-bug-report`'s size-up (fetch the repo, check recent drift) → solo-vs-sub-agent branch logic as-is, to get the target service's entity graph (creatable objects and their relationships) and recent code changes. Skip re-investigating if this is already available in the same session.
1. **Design the entity lifecycle.** From the entity graph, pull out creatable objects (e.g., collection/file/entry/project) and chain them create → verify → delete. If there's a parent-child dependency, delete children first, and check whether deleting a parent is blocked while children remain. After the per-entity chains, add one or two explicit **cross-module flow** steps (e.g., does an object created in module A show up in module B's list/search/dashboard?). "Full-cycle" really means the connections between modules are alive, and per-module create → verify alone can't show that. Keep permission boundaries to roughly one check at smoke scope (does an account without permission get blocked?) and push per-role permission matrices to the regression track.
2. **Confirm name uniqueness.** Check the code for whether each entity's name field actually enforces uniqueness (e.g., grep for an "already exists"-style error message). Only fields that do get a timestamp suffix, and cleanup uses that prefix for a bulk delete — reflect this in the spec.
3. **Write the execution meta-rules.** Fill in these standard sections: result-logging format (pass/fail/NT verdicts, a step/verdict/note/evidence table, no guessing or interpretation), evidence-collection rule (failed/NT rows must carry an observed string or screenshot), wait policy (poll interval/timeout/on-timeout log as failure and move on/allowed to work on other sections while waiting), side-effect guards (fixed test account, existing data is untouchable, original value recorded + restored for any shared setting you change), file-upload technique (see core principles above), and any tool-specific traps hit while handling input fields (if any).
4. **Write the known-normal-behaviors list.** Pre-record anything found during service understanding that "looks like a bug but is intended" (mock features, no cascade delete, per-tab filters saved independently, a button that's supposed to be disabled under some condition, features switched off by an environment flag or plan tier, etc.) to head off false positives.
5. **Finish the v1 deliverable.** Using the results of steps 1-4, produce a spec — execution rules + naming + test order (create → verify → cross-module → cleanup) + known-normal behaviors — in a form you can paste directly into Claude in Chrome. Leave the UI anchor glossary and the "known issues, don't re-report" section empty or as placeholders. Run the verification checklist below before handing it over.
6. **Fold in run results (v2+).** When the user brings back results from actually running v1, classify each failure as "a real bug" vs. "a browser-agent/tool limitation" (see core principles). Absorb the latter into the execution rules or known-issues section, and fill the UI anchor glossary with the exact labels/menu locations/DOM structure confirmed during the run. Repeat this cycle until the spec stabilizes.

## Verification checklist (before handing over the deliverable)

- [ ] No step goes through an OS file dialog (every upload targets `input[type=file]` directly).
- [ ] Every created entity has a code-confirmed uniqueness basis and a prefix-based cleanup step.
- [ ] No step can touch existing data, other members, or accounts outside your control.
- [ ] Every shared setting the test changes has a paired record-original-value step and restore step.
- [ ] The final-output section explicitly says cleanup failures go at the very top of the report.
- [ ] Every assertion is grounded in current behavior confirmed by code/spec. Features confirmed only by a merge are conditional steps.
- [ ] Every step was checked for "can the condition actually be observed at this point?" (an earlier step hasn't already removed what you wanted to observe).
- [ ] Broad smoke scope and deep regression scope aren't mixed (permission matrices, detailed state transitions go to regression).
- [ ] For a v1, the UI anchor glossary and known-issues sections are empty or placeholders (nothing filled in without a real run).
- [ ] The results table has pass/fail/NT + an evidence column, and app defects are reported separately from browser-agent limitations.

## Reference

- For the deliverable's 6-section template (execution rules / naming / UI anchor glossary / known-normal-behaviors & issues / test order / final output) plus a worked example, see `references/template-and-example.md`.
- Sibling skills: `functional-tc-authoring` (code → a QA test-case sheet a human runs by hand), `delta-bug-report` (code diff → a bug-issue doc for engineers). This skill picks up the "service understanding" those two skills already produce and goes the other direction: the executor is an AI browser agent, not a person. That's the biggest difference from its siblings, and it's why half the deliverable is "meta-rules to keep the AI agent from wandering off."
