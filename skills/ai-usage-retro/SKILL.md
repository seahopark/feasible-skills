---
name: ai-usage-retro
description: |
  Runs a recurring feedback loop on how someone actually uses their AI agents: reads their real conversations (how they hand work over) and the tool's real state (settings, sessions, automations, versions), then suggests better prompts, model choices, setup changes, and newly available features, and asks for their reaction.
  Use this when someone wants a periodic review of their AI workflow, asks "am I actually using AI well," wants prompt, model, or setup suggestions grounded in their own history, asks what a tool upgrade made possible, or wants to set up a weekly/monthly retro loop.
---

# Skill: AI Usage Retro

## What this is for

Most people can't accurately describe their own AI usage: self-report is edited memory. Wins get remembered bigger, misses get rationalized away.

This skill is not an audit. It is a weekly (or monthly) feedback loop that looks through two lenses at what actually happened:

- **Lens A, how you hand work over:** the person's own messages in recent agent conversations.
- **Lens B, how the tool is set up:** settings, conversation and model habits, skills, automations, versions and what's new.

It then suggests what to try next (a prompt change, a model for a task, a setup change, a new feature) and asks how last time's suggestions went. The goal is that the person learns what is possible and improves a little each cycle.

**Relationship to `openclaw-usage-review`:** this skill is its follow-up. That skill reviews the tool side for OpenClaw setups; running it showed that part of the problem was in how requests were handed to the agent. This skill keeps that tool-side review (Lens B, tool-agnostic, with the OpenClaw commands in [references/openclaw.md](references/openclaw.md)) and adds Lens A. `openclaw-usage-review` still works on its own.

Do not rank "what hurts most." Many people have no felt pain to point at. Lead with evidence-based suggestions. Merely precautionary items (for example, a memory file nearing a size limit) go in a separate "preventive" list.

---

## Core principle: artifacts over narrative

Pull the real, unedited record and analyze that, not the person's description of it. Sources, in priority order (use what is accessible; don't ask the person to paste history that could be pulled):

1. **The person's own messages** in at least five recent conversations. Read the raw text of their turns. Note which conversations and message ranges you read; many tools return only the tail of a long conversation.
2. **Tool state:** conversation/model metadata, skills, automations, memory, version, cost (see Lens B).
3. Agent memory/notes files and git commits changed since the last retro.
4. Issue tracker, docs/wiki, calendar, only if connected.

---

## Ground rules

- **Read and suggest only.** Changes to settings, upgrades, restarts, deletions, and memory/notes files need the person's approval. The retro changes nothing except its own state file and report.
- **Keep a state file.** One small file holds the person's operating principles, a dated do-not-repropose list, `lastCheckedVersion`, and each suggestion's status (suggested / applied / rejected). Read it before proposing anything and drop matches. When the person's reply to the closing question rejects or corrects a suggestion, record it with the date and reason. Re-raise a rejected item only with data showing conditions changed.
- **Confirm before recommending.** Confirm every suggested feature at its source (docs or release page). Anything unconfirmed is labeled "unverified" and not recommended.

---

## Process

### Step 1: Gather evidence
Pull a representative sample from the sources above for the review period; if the window since the last retro is short, widen it (for example, two weeks). Read the previous retro and check its "things to try" against the conversations: were they tried?

### Step 2: Lens A, the person's messages
- **Corrections and pushback**, and the assumption the agent made just before each. (Most trace back to missing scope or context in the request, not a badly written request.)
- **Plans the person had to ask to have re-explained.**
- **Decisions made somewhere else** and reported to the agent only later.
- **What a request that landed in one pass looked like.** Find at least one and quote it; what the person does well belongs in the retro.

Compare with the previous retro: up or down?

### Step 3: Lens B, the tool
Follow [references/tool-side-review.md](references/tool-side-review.md): usage signals, versions, suggestion candidates, and a short breakage check. For OpenClaw, use the commands in [references/openclaw.md](references/openclaw.md). If a separate tool-side report exists, read its latest one as extra evidence and say its date.

Up to five candidates go in the evidence section; **at most one** enters this week's "things to try," so setup housekeeping doesn't crowd out prompt suggestions.

### Step 4: Check against what's newly possible
Compare current usage with capabilities that exist now but may not have when the person's habits formed. If a tool's version changed since the last retro, read every release note in between, not just the latest, from official pages with source URLs. If a search tool fails, fetch the release page instead of retrying. Model suggestions must not assert performance differences without evidence; with a small sample, say "try this and see."

### Step 5: Write the retro in five parts
Short, direct, prose over bullets, no praise opener, no hedging.

1. **One-line conclusion.** How the week of AI use went.
2. **The week's story, 2–3 sentences.** What they were trying to do, where it went sideways, where it went well, through real conversation names.
3. **One scene.** One of their requests next to a better version, plus one line on what changes.
4. **1–2 things to try this week.** Why and how, one line each. Model, setup, and new-feature suggestions go here.
5. **One question.** Usually whether last week's suggestion was tried and how it went.

### Step 6: Keep the evidence separate
Save a file with the five parts at the top and an "Evidence" section below: conversations read and ranges, counts, prompt/model/feature analysis, the Lens B results (usage numbers, versions and hold/upgrade call, breakage, candidates with status), decisions made elsewhere but missing from notes, preventive items, blind spots. No internal notation (counts strings, file names, session keys, bug numbers) in the message itself. Where there is no evidence, say so and ask; don't guess.

### Step 7: Verify before sending
Pick two suggestions at random and recount their evidence against the live source (re-read the quote, recount the conversations, re-check the date fields: use the one that reflects real activity, not one that migrations or bulk updates rewrite). Fix mismatches before sending. Confirm every recommended feature has a source URL.

### Step 8: Deliver as written, and close the loop
For a scheduled run, send the message directly through your messaging tool, not through another session that may paraphrase it, and keep progress narration out of it. If sending fails, note it at the top of the saved file and in the final response. Recommend a cadence (weekly if things change fast, monthly if stable) and set up a recurring trigger if a scheduler exists. Next time, re-check whether the "things to try" were tried.

---

## Output template

```markdown
# AI Usage Retro — [date]

## One-page summary
**1) One-line conclusion** [...]
**2) The week's story** [2–3 sentences]
**3) One scene** Your request: "[quote]" / Better version: "[rewrite]" / What changes: [one line]
**4) Try this week** [thing]: why — [...]; how — [...]
**5) Question** [e.g., last week's suggestion: did you try it?]

---

## Evidence
### Conversations read (title, range, model)
### Prompt / direction analysis
### Model suggestions (small samples marked "try and see")
### Tool-side review (usage numbers, version and hold/upgrade call, breakage, candidates with status)
### New features (source URLs; unverified ones marked, not recommended)
### Decisions made elsewhere but missing from notes
### Preventive items (no confirmed cost yet)
### Blind spots and what was not read
```
