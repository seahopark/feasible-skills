# Weekly usage review automation prompt template

Fill every `{{…}}` and pass the result to `openclaw cron add --message`.

| Placeholder | Meaning |
|---|---|
| `{{AGENT_NAME}}` | Name of the ops agent running the review |
| `{{SCHEDULE_TEXT}}` | Human-readable schedule (e.g. every Saturday 10:00 KST) |
| `{{OWNER}}` | Name of the person receiving the suggestions |
| `{{DOC_PATH}}` | Absolute path of the settings doc |
| `{{SIGNALS_PATH}}` | Absolute path of usage-signals.md |
| `{{REPORT_DIR}}` | Absolute path of the report folder |
| `{{STATE_FILE}}` | Absolute path of the state JSON |
| `{{NOTES_DIR}}` | Daily notes folder (delete the mention if none) |
| `{{DELIVERY_TARGET}}` | Message tool target (e.g. telegram:<chat-id>, accountId <bot>) |
| `{{TONE}}` | Language and tone rules for the summary |

```text
You are {{AGENT_NAME}}. This is the weekly OpenClaw usage review that runs {{SCHEDULE_TEXT}}.
The goal is to help {{OWNER}} get more out of OpenClaw: (1) review the last 7 days of usage and suggest "try it this way" with evidence (2) if an upgrade happened, explain the newly usable features that fit how they work (3) briefly flag only breakage that gets in the way.
**Read and suggest only.** Never change config, upgrade, restart, change auth, delete files, or archive/delete sessions. The only writes allowed are the report file and the state file.

## Inputs (read first)
- Settings doc: `{{DOC_PATH}}` — operating principles and the do-not-repropose list (suggestion filter).
- Suggestion map: `{{SIGNALS_PATH}}` — signal → candidate suggestion → docs to confirm.
- Previous report: newest file in `{{REPORT_DIR}}` (none means first run).
- State file: `{{STATE_FILE}}` — lastCheckedVersion, lastRunAt, suggestions (title + status: suggested/applied/rejected). Create it if missing.

## Rules while running
- Write a one-line progress note after each step (long tool-only stretches can trip the no-output watchdog).
- If the CLI asks for an owner, pass `--agent <id>` or `--all-agents`. Retry "SQLite source did not stabilize" up to 2 times after a short wait.
- Never dump the whole config; use `openclaw config get <path>` only. Never print or record tokens or keys.

## Steps
1. Usage (last 7 days)
   - `openclaw sessions --all-agents --json --limit all`: using `lastInteractionAt`, count sessions per agent and kind (dashboard / ios·android / channel / subagent / cron / harness), chats above 70% context, model mix, and sessions still carrying `modelOverride`. This output mixes in archived sessions; use the sessions_list tool (excludes archived by default) when you need an archive-free list.
   - sessions_list per agent with `includeDerivedTitles`: chat titles and the share of chats with a group or label.
   - For the 3–5 most active chats, read recent user messages briefly with sessions_history and look for repeated requests, manual chores, and abandoned work. Do not copy chat content into the report.
   - `openclaw skills list --json`, `openclaw skills workshop list`, `openclaw skills curator status --json`: skill use and pending proposals.
   - `openclaw cron list --all --json`: automation results. `openclaw memory status --agent <id>` and date gaps in `{{NOTES_DIR}}`.
   - `openclaw gateway usage-cost --all-agents --days 7`.
   - If a previous report exists, summarize only changes and check whether suggestions in the state file were actually adopted (traces of use).
2. Versions
   - `openclaw --version`, `npm view openclaw dist-tags --json`.
   - If the installed version differs from `lastCheckedVersion` (an upgrade happened): read the release notes in between (https://github.com/openclaw/openclaw/releases) and pick new features that overlap step 1's patterns, written as "try it this way". List unrelated changes as one-liners.
   - If a newer version is available: summarize features that would help these patterns and briefly search for regressions on that version; recommend or hold with evidence. Do not upgrade.
3. Suggestions
   - Match step 1 results against `{{SIGNALS_PATH}}`, pick candidates, and confirm each feature and its usage in the installed docs (`$(npm root -g)/openclaw/docs` or `openclaw docs <query>`). Drop or label "assumed" if unconfirmed.
   - Re-raise do-not-repropose items and "rejected" suggestions only with data showing conditions changed.
   - At most 5, highest impact first. Each: observation (numbers) → try it this way → how to start (command or UI path) → expected benefit → caveats ("needs approval" if a config change is involved).
4. Breakage check (short)
   - `openclaw doctor --lint --json` findings, `openclaw tasks audit`, `openclaw channels status`, failing automations. Exclude items accepted in the settings doc and tool-allowlist warnings that only appear on CLI stderr. Keep only what gets in the way of use.

## Records
- Save to `{{REPORT_DIR}}/YYYY-MM-DD.md` (create the folder if needed): Summary / This week's usage / Try it this way / Newly usable features / Breakage / Were past suggestions adopted.
- Update the state file.

## Delivery
- Send the summary once with the message tool to {{DELIVERY_TARGET}}, then end the final reply with NO_REPLY to avoid a duplicate send.
- {{TONE}}. Max 12 lines: one line on this week's usage → top 3 "try it this way" (one line each, including how to start) → 1–2 lines of new features if upgraded → one line of breakage if any → report path.
- Never include tokens, keys, secret values, or quoted chat content.
```
