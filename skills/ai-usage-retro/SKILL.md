---
name: ai-usage-retro
description: |
  Runs a recurring feedback loop on how someone actually uses their AI agents/tools: reads real conversation transcripts and artifacts (not self-report), then suggests better prompts, model choices, and newly available features, and asks for the person's reaction.
  Use this when someone wants a periodic review of their AI workflow, asks "am I actually using AI well," wants prompt or model suggestions grounded in their own history, or wants to set up a weekly/monthly retro loop for their own AI-assisted work.
---

# Skill: AI Usage Retro

## What this is for

Most people can't accurately describe their own AI usage. They know they used AI "a lot," but when it worked and why comes out fuzzy, because self-report is edited memory: wins get remembered bigger, misses get rationalized away.

This skill is not an audit. It is a weekly (or monthly) feedback loop: read what actually happened, show the person one concrete moment where a prompt could have been better, suggest what to try next (a prompt change, a different model for a task, a newly released feature), and ask how last time's suggestions went. The goal is that the person learns what is possible and improves a little each cycle.

**Relationship to `openclaw-usage-review`:** this skill is the follow-up to [`openclaw-usage-review`](../openclaw-usage-review/SKILL.md). That skill reviews the tool side (settings, sessions, versions) for OpenClaw setups. Running it showed that part of the problem was in how requests were being handed to the agent, so this skill reviews that side too and works with any agent. If you run a tool-side review as well, see the note in Step 3.

Do not rank "what hurts most." Many people have no felt pain to point at. Lead with suggestions grounded in evidence. Things that are only precautionary (for example, a memory file growing toward a size limit) go in a separate "preventive" list, not mixed into what is going wrong.

---

## Core principle: artifacts over narrative

Don't ask the person to describe their AI usage and analyze that description. Pull whatever real, unedited record exists and analyze that.

**Why:** a verbal account is already a compressed, flattering summary. A conversation with three false starts, a plan the person asked to have re-explained, a decision made elsewhere and passed along later: that raw material shows the pattern.

**Sources, in priority order** (use what is actually accessible; skip the rest, and don't ask the person to paste history that could be pulled directly):

1. **The person's own messages in recent agent conversations** (at least five conversations from the review period). Read the raw text of the person's turns, not a summary. Note which conversations and which message ranges you read; many tools only return the tail of a long conversation, so say so.
2. Agent memory/notes files changed since the last retro (for example, how many new "correction" notes were written).
3. Git commits and documents in the person's repos since the last retro.
4. Per-conversation model and token metadata, if the tool exposes it.
5. Issue tracker, docs/wiki, calendar, only if connected.

---

## Ground rules

- **Read and suggest only.** Changes to settings, upgrades, restarts, deletions, and memory/notes files need the person's approval. The retro itself changes nothing except its own state file (below).
- **Keep a do-not-repropose list.** Keep one small state file with the person's operating principles and a dated list of suggestions they rejected. Read it before proposing anything and drop matches. When the person's answer to the closing question rejects or corrects a suggestion, add it with the date and the reason. This is what stops the same suggestion coming back every week.
- **Confirm before recommending.** Every suggested feature must be confirmed at its source (official docs or release page). Anything you couldn't confirm is labeled "unverified" and not recommended.

---

## Process

### Step 1: Gather evidence

Pull a representative sample from the sources above. Prioritize the review period. If the window since the last retro is short, widen it (for example, to two weeks). Read the previous retro file and check its "things to try" list against the conversations: did the person try them?

### Step 2: Look at the person's messages for four things

- **Corrections and pushback**, and the assumption the agent made just before each one. (Most corrections trace back to missing scope or context in the request, not to a badly written request.)
- **Plans the person had to ask to have re-explained** ("what exactly are you going to do?").
- **Decisions made somewhere else** (issue tracker, code, another tool) and reported to the agent only later.
- **What a request that landed in one pass looked like.** Find at least one and quote it. What the person does well belongs in the retro too.

Compare against the previous retro: did these go up or down?

### Step 3: Check against what's newly possible

Compare current usage with capabilities that exist now but may not have when the person's habits formed: new features in the tools they already use, integrations or delegation patterns they have but don't use, manual workarounds for limits that no longer exist.

**If you also run a tool-side review** (for example `openclaw-usage-review` for OpenClaw setups), read its latest report as one more evidence source and take at most one of its findings into this week's "things to try." The rest stays in the evidence section, so tool housekeeping doesn't crowd out the prompt suggestions. Say which date that report is from.

If a tool's version changed since the last retro, establish the previous and current versions and read every release note in between, not just the latest. Read release notes directly from official pages and include the source URL. If a search tool fails, fetch the official release page instead of retrying. Anything you could not read at its source is marked "unverified" and is not recommended. Model suggestions must not assert performance differences without evidence; with a small sample, phrase them as "try this and see."

### Step 4: Write the retro in five parts

Write the person-facing summary in this shape. Short, direct, prose over bullet lists, no praise opener, no hedging.

1. **One-line conclusion.** How the week of AI use went.
2. **The week's story, 2–3 sentences.** What they were trying to do, where it went sideways, where it went well, told through real conversation names and moments.
3. **One scene.** Quote one of their requests next to a better version, plus one line on what changes. (Also fine to pair it with a request that worked.)
4. **1–2 things to try this week.** For each: why, and how, one line each. Model and new-feature suggestions go here as "things to try."
5. **End with one question.** Usually whether last week's suggestion was tried and how it went. This is how the person's reaction enters the loop.

### Step 5: Keep the evidence separate from the message

Save a full file with the five-part summary at the very top and, below it, an "Evidence" section: the conversations read and their ranges, the counts, the prompt/model/feature analysis, decisions found in commits but missing from memory, preventive items, and blind spots. The top page alone should convey the flow.

Keep internal notation out of the message itself: no counts strings, file names, session keys, or bug numbers. Those belong in the evidence section only.

**Blind spots:** where there is no evidence, say so and ask; don't fill the gap with a guess. Common ones: conversations you could only read the tail of, tools whose history isn't accessible to you, and whether an output was actually used afterwards.

### Step 6: Verify before sending

Pick two suggestions at random and recount the evidence behind each against the live source (for example, re-read the quoted message, re-count the conversations, re-check the date fields you relied on: use the timestamp that reflects real activity, not one that migrations or bulk updates rewrite). If the numbers or quotes don't match, fix them before sending. Confirm every feature you recommend has its source URL.

### Step 7: Deliver it as written, and close the loop

If the retro runs on a schedule, deliver the message directly through your messaging tool. Don't route it through another session that may paraphrase it or append unrelated events, and don't let progress narration ("collecting messages…") leak into the message. If sending fails, note that at the top of the saved file and in the final response.

Recommend a cadence (weekly if tooling and habits are changing fast, monthly if stable) and set up a recurring trigger if a scheduler is available. Next time, re-check whether the "things to try" were actually tried.

---

## Output template

```markdown
# AI Usage Retro — [date]

## One-page summary

**1) One-line conclusion**
[...]

**2) The week's story**
[2–3 sentences with real conversation names]

**3) One scene**
Your request: "[quote]"
Better version: "[rewrite]"
What changes: [one line]

**4) Try this week**
- [thing]: why — [...]; how — [...]
- [thing]: why — [...]; how — [...]

**5) Question**
[e.g., last week's suggestion — did you try it, and how did it go?]

---

## Evidence

### Conversations read
[title — message range — model]

### Prompt / direction analysis
[quotes, better versions, patterns that worked]

### Model suggestions
[task type — model used — outcome — what to try; mark small samples as "try and see"]

### New features (with source URLs; unverified ones marked and not recommended)

### Decisions made elsewhere but missing from notes/memory

### Preventive items (no confirmed cost yet)

### Blind spots and what was not read
```
