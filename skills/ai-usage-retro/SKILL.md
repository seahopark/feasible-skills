---
name: ai-usage-retro
description: |
  Periodically evaluates how someone is actually using their AI agents/tools, grounded in real artifacts rather than self-report, and turns the findings into a concrete improvement loop matched against what's newly possible now.
  Use this when someone wants a periodic self-review of their AI workflow, asks "am I actually using AI well," wants to check their usage against newly released agent/tool capabilities, or wants to set up a recurring retro loop for their own AI-assisted work.
---

# Skill: AI Usage Retro

## What this is for

Most people can't accurately describe their own AI usage patterns — they know they used AI "a lot," but explaining *when it worked and why* comes out fuzzy. That's because self-report is edited memory: wins get remembered bigger, misses get rationalized away.

This skill runs the evaluation against real artifacts instead — issue tracker history, docs, git commits, session/chat transcripts, whatever's actually connected — so the analysis isn't built on an edited narrative. It then checks that usage against what's newly available (new agent features, new integrations, new skills) and turns the gap into a short, concrete action list. Run it on a recurring cadence, not once.

---

## Core principle: artifacts over narrative

Don't ask the person to describe their AI usage from memory and analyze that description. Pull from whatever real, unedited record already exists and analyze that instead.

**Why this matters:** a verbal account is already a compressed, self-flattering summary. A ticket that got reverted, a doc that went through five revisions, a session transcript with three false starts before the right prompt — that's the raw material that actually shows the pattern. The gap between "what I think I did" and "what the record shows" is itself useful signal.

**How to apply it:** before analyzing anything, check which of these sources are actually accessible (via MCP, API, or direct file access) and use whichever exist. Skip what isn't connected — don't ask the user to manually paste in history that could otherwise be pulled directly.

- Issue tracker (Linear, Jira, GitHub Issues, etc.) — tickets written, comments, resolution patterns
- Docs / wiki (Confluence, Notion, Obsidian, etc.) — what got documented, revision history if visible
- Git history — commit messages, PR descriptions, how AI-assisted changes are flagged (if at all)
- Session/chat transcripts with the AI agent itself — actual prompts sent, how well-scoped they were, how many turns it took to get a usable result
- Calendar/meeting notes, if relevant to how AI fits into the work rhythm

---

## Process

### Step 1: Gather evidence

Connect to whatever sources are available (Step 0 above) and pull a representative sample — don't try to ingest everything if the volume is large. Prioritize:
- Recent activity (last review period) for current-state accuracy
- A few older artifacts for a before/after comparison if this isn't the first retro

### Step 2: Extract actual usage patterns

From the evidence, identify:
- **What's actually delegated to AI** vs. done manually — not what the person assumes they delegate
- **How requests are structured** — vague one-liners that took multiple rounds to clarify vs. well-scoped requests that landed in one pass
- **What integrations/tools are wired up** and how much they're actually used vs. sitting idle
- **Where domain knowledge got captured** (as a skill, a template, a documented convention) vs. where it stayed tacit and had to be re-explained each time

### Step 3: Check against what's newly possible

This is the part that makes it a *retro* rather than a one-time audit. Compare current usage against capabilities that exist now but may not have when the person's habits formed:

- New features in the agent/tool(s) already in use (check release notes/changelogs if accessible, or ask the user what's changed since the last review)
- Capabilities the person has access to but isn't using (e.g. a new integration, a subagent/delegation pattern, a scheduling/automation feature)
- Patterns that were manual workarounds for a limitation that no longer exists

Flag anything here as a candidate for adoption, not a mandate — the point is to surface options, not prescribe.

### Step 4: Produce the retro output

Structure the output into four parts. Don't skip the fourth — it's the part that actually closes the loop:

1. **Working well, with evidence** — cite the specific artifact each finding is based on, not just an impression
2. **Needs improvement, with reasoning** — be specific about the cost (maintenance burden, repeated re-explanation, dropped context), not just "could be better"
3. **Blind spots** — areas where the evidence is simply absent. **Don't fabricate an assessment for these.** State plainly that there's nothing to go on, and ask the person directly instead of guessing. This is usually the most useful part of the retro: what's missing from the record is itself a finding (e.g. "no artifact captures how decisions actually got made" is a signal about what isn't being documented, not a gap in the analysis).
4. **Concrete next actions** — a short list, each tied to one of the findings above. Distinguish what to adopt from what to retire (a workflow habit that made sense under old constraints but doesn't anymore).

### Step 5: Ask back before finalizing

If gaps from Step 4's "blind spots" section would meaningfully change the retro's conclusions, ask the person a short list of targeted questions rather than filling them in with assumptions. This mirrors how a good performance review works — the reviewer doesn't invent context they don't have.

### Step 6: Close the loop

End with a recommended cadence for the next retro (monthly and quarterly both work well in practice — monthly if the person's tooling is changing fast, quarterly if it's stable) and note what specifically should be re-checked next time (usually: did the adopted actions from this retro actually get adopted?). If the person has a scheduling/automation tool available, suggest setting up a recurring trigger at that cadence instead of relying on remembering to run it manually.

---

## Output template

```markdown
# AI Usage Retro — [date]

## Evidence sources used
- [source 1, date range covered]
- [source 2, date range covered]
(note any sources that exist but weren't accessible, and why)

## Working well
- [Finding] — evidence: [specific artifact/citation]
- [Finding] — evidence: [specific artifact/citation]

## Needs improvement
- [Finding] — cost: [what this is actually costing — time, rework, dropped context]
- [Finding] — cost: [...]

## Blind spots (no evidence found — not analyzed, just flagged)
- [Area with no supporting artifact]
- [Area with no supporting artifact]

## Questions to close the gaps above
1. [Targeted question]
2. [Targeted question]

## Next actions
- Adopt: [specific new capability/pattern to start using]
- Retire: [specific habit that no longer earns its cost]
- Keep doing: [what's already working, don't disrupt it]

## Next review
- Cadence: [monthly/quarterly]
- Specifically re-check: [whether adopted actions actually got adopted, plus anything flagged as evolving]
```
