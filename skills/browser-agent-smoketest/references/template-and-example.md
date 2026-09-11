# Deliverable template + worked example

## Template skeleton

```
# {service name} full-cycle smoke test

> {login URL}, start already logged in.
> The goal is a quick check that things broadly work, not detailed bug hunting.

---

## A. Execution rules

### A-1. Result logging format
Per step: step / verdict (binary pass/fail, when in doubt fail) / note (actual actions + observations only, no guessing/interpretation/suggestions)

### A-2. Wait policy
| Target | Poll interval | Timeout | Pass condition |
Timeout = log as failure and move to the next step. OK to work on other sections while waiting.

### A-3. Side-effect guards
Approver/assignee/mention targets all go to one fixed test account. Never touch existing data — only what this run created.

### A-4. Handling input fields (filled in from v2 onward)
Traps hit with the tool itself while running, and the workaround. E.g., an alternative technique when a standard keyboard shortcut doesn't register.

### A-5. File upload
For Claude in Chrome: never click the upload button (can't see the native OS dialog) → target `input[type=file]` directly; for React, dispatch `input`+`change` events with `bubbles:true`. Also specify a fallback for when no file is accessible.

---

## B. Test data naming
```
BASE = {prefix}_{MMDD_HHmm}
```
| Object | Name | Uniqueness constraint |
Cleanup deletes everything starting with `{prefix}_` in bulk (including leftovers from earlier runs).

---

## C. UI anchor glossary (filled in from v2 onward — needs at least one real run to pin down)
Exact button labels, menu locations, icon cluster order per screen. A cache that cuts down navigation cost from having to screenshot-and-guess every time.

---

## D. Known-normal behaviors / known issues

### D-1. Known-normal behaviors (don't report these as bugs)
"Looks like a bug but is intended" behaviors confirmed by code during the service-understanding step.

### D-2. Known issues (already confirmed in v1 — don't re-report, just work around them) (filled in from v2 onward)
Browser-agent/tool-limitation bugs found during an actual run, and how to work around them.

---

## E. Test order
Per entity: create → verify → (next entity) → ... → cleanup (reverse creation order, delete children first, finish with a prefix search confirming zero results)

---

## F. Final output
A per-step results table + a separate list of failures/unexpected behavior + flag any cleanup-step failure at the very top (it's a hard failure that blocks the next run)
```

## What each section is for, illustrated with a worked example

### Why A-2 (wait policy) matters
One file-parsing screen we tested showed an ETA like "About 12 min left," but it often actually finished in 15 seconds (the ETA just wasn't accurate). If you tell the agent nothing but "wait until it says Done," with no polling + timeout, it'll take the inaccurate ETA literally and waste the whole turn waiting.

### A-4 (input-field traps) — an example of something you only find by running it
Only after actually running v1 did this surface: "this service's input fields don't respond to Ctrl+A for select-all, so trying to clear an existing value with Ctrl+A then typing ended up appending instead — producing something like `test_auto_editedtest_auto`." You can't catch this by reading code; it only shows up once you actually run it — which is exactly why the v1 → v2 revision cycle exists.

### D-1 vs. D-2
- D-1 (known-normal behavior) is **the service's own design**. Example: "collection deletion is blocked while files remain inside it — that's intended." This comes out of the code/spec-understanding step.
- D-2 (known issues) is **a limitation of the execution environment/tool**. Example: "the Dashboard's ask/search input has animated placeholder text written directly into the input's `value`, so typing normally interleaves with it → clear it fully with End+Backspace first, then type." This only shows up from an actual run.
Mixing the two blurs "this is a bug report" vs. "this is just a workaround," so keep them separate.

### B (naming) — a self-healing design
In one case, only the collection-name field actually enforced uniqueness ("This name already exists."-style error) — project titles and folder names had no such check. So every field still got a timestamp suffix, but the spec explicitly called out exactly which field blocks cleanup if it fails (the collection name), so the executing agent would flag that specific failure at the top ("especially: if collection deletion fails, escalate it — it blocks the next run" in section F).

### E (test order) — a dependency-ordering mistake
In one run, a "does deleting a parent folder get blocked while a project is still inside it?" step was supposed to observe that condition — but by the time that step ran, an earlier step had already deleted all the projects, so there was nothing left inside to actually trigger the block. When ordering steps, check at every step whether the condition you actually want to observe is still observable at that point.
