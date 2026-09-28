# Lens B: tool-side review (tool-agnostic)

Read the tool's real state, then suggest "try it this way" with evidence. Use whatever your tool exposes; for OpenClaw the exact commands are in [openclaw.md](openclaw.md).

## Inputs
- State file: `lastCheckedVersion`, `lastRunAt`, suggestions with status (suggested / applied / rejected), do-not-repropose list.
- Previous report (none means first run).

## 1. Usage (review period)
Measure, per agent and per surface (desktop, mobile, chat channel, subagent, scheduled job):
- Conversation counts, and conversations that grew past ~70% of the context window.
- Model mix, and conversations still pinned to a model instead of following the default.
- Titles/groups/labels: share of conversations that have them.
- Skills: which exist, which were used, which proposals are pending.
- Automations: which ran, which failed quietly or never delivered.
- Memory/notes: gaps in daily notes, whether memory search is used.
- Cost/usage, if exposed.
- Sample the 3–5 most active conversations' recent user messages briefly; look for repeated requests, manual chores, abandoned work. Put patterns, not conversation content, in the report.
- If a previous report exists, summarize only changes and check whether earlier suggestions left traces of use (were they applied?).

## 2. Signals → candidate suggestions
Every row is a candidate. Confirm in the tool's docs that the feature exists in the installed version before suggesting it.

| Signal | Candidate suggestion |
|---|---|
| Conversations grow past ~70% of context | Hand off a summary into a fresh conversation, compact, move side questions elsewhere |
| The same kind of request repeats across days | Turn it into an automation; capture the procedure as a skill |
| Conversations pile up without titles or groups | Groups/labels; archive finished ones |
| Heavy subagent use | Goal tracking, progress cards, parallel lanes |
| Long tasks delegated from a chat channel | Progress streaming, completion notices |
| Conversations pinned to a model | Reset to follow the configured default |
| Skills unused or proposals piling up | Sharpen skill descriptions (trigger phrases), review pending proposals, retire unused skills |
| Gaps in daily notes, memory search unused | Turn on active memory / memory search, check the checkpoint job |
| Automations fail quietly or results never arrive | Failure alerts, delivery review |
| Browser, file, or code work directed by hand every time | Point to the matching tool or plugin |

## 3. Versions
- Get the installed version and the latest available.
- If the installed version differs from `lastCheckedVersion` (an upgrade happened): read the release notes in between and keep only the new features that overlap the usage patterns above, written as "try it this way". List unrelated changes as one-liners.
- If a newer version exists: summarize what it would add for these patterns, search briefly for open regressions on that version, and recommend or hold with evidence. Never upgrade.

## 4. Suggestions
- At most five, highest impact first. Each: observation (numbers) → try it this way → how to start (command or UI path) → expected benefit → caveats ("needs approval" if a config change is involved).
- Drop do-not-repropose items and anything unconfirmed.

## 5. Breakage check (short)
Lint/health findings, failing automations, channel status. Keep only what gets in the way of use. Exclude items the person already accepted.

## Records
Update the state file: `lastCheckedVersion`, `lastRunAt`, each suggestion's status. Save the tool-side results into the retro's Evidence section.
