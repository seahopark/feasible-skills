# Usage signals → "try it this way" candidates

Every row is a **candidate**. Before suggesting it, confirm in the installed docs (`$(npm root -g)/openclaw/docs`, or `openclaw docs <query>`) that the feature exists in the current version and how to use it. If you can't confirm, drop it or label it "assumed". Skip anything on the settings doc's do-not-repropose list.

| Signal | How to measure | Candidate suggestion | Docs to confirm |
|---|---|---|---|
| Chats grow past 70% of context | `totalTokens / contextTokens` in sessions JSON | Hand off a summary into a fresh chat, `/compact`, move side questions to a side question | `concepts/compaction.md`, `tools/btw.md` |
| The same kind of request repeats across days | Session titles (sessions_list `includeDerivedTitles`), daily notes | Turn it into an automation; capture the procedure as a skill (`/learn`, Skill Workshop) | `automation/`, `tools/self-learning.md` |
| Chats pile up without titles or groups | Share of `group` / `label` in sessions_list | Sidebar groups and labels; archive finished chats | `web/control-ui/` |
| Heavy subagent use | Subagent session count and parent sessions | Goal tracking, progress cards, parallel work lanes | `tools/goal.md`, `tools/progress-card.md`, `tools/subagents.md` |
| Long tasks delegated from a chat channel | Turn length and tool use in channel sessions | Progress streaming, rich messages, completion notices | `concepts/progress-drafts.md`, `channels/<channel>.md` |
| Chatting from the mobile app | `ios-` / `android-` sessions | Node features such as voice, camera, location | `nodes/`, `platforms/` |
| Sessions still pinned to a model | `modelOverride` / `providerOverride` in sessions JSON | `/model default` to follow the configured default | `concepts/models.md` |
| Skills go unused or proposals pile up | `openclaw skills list --json`, `openclaw skills workshop list`, `openclaw skills curator status --json` | Sharpen skill descriptions (trigger phrases), review pending proposals, retire unused skills | `tools/skills.md`, `tools/skill-workshop.md` |
| Gaps in daily notes, memory search unused | Missing note dates, `openclaw memory status` | Active memory, memory_search, checkpoint job check | `concepts/active-memory.md`, `concepts/memory-search.md` |
| Automations fail quietly or results never arrive | `openclaw cron list --all --json`, `openclaw tasks audit` | failureAlert, delivery review | `automation/cron-jobs.md` |
| Browser, file, or code work directed by hand every time | Recent chat sample | Point to the matching tool or plugin | `tools/index.md`, `concepts/features.md` |

## Reading a chat sample
- Pick only the 3–5 most active chats of the last 7 days (by last interaction and tokens).
- Read recent user messages briefly with `sessions_history` and look for manual chores, repeated explanations, and abandoned work.
- Put patterns, not chat content, into reports and notifications.
