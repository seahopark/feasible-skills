---
name: openclaw-usage-review
description: |
  Review how someone actually uses OpenClaw and suggest "try it this way" improvements, explain what became usable after an upgrade, or set up a weekly usage review automation.
  Use when someone asks "am I using OpenClaw well?", "look at my usage this week and recommend things", "I upgraded — what can I use now?", "set up a weekly review", or right after changing a setting.
  Produces: usage-based suggestions (evidence + how to start), per-version new-feature notes, a read-only weekly review automation, and a settings doc that filters suggestions.
---

# OpenClaw Usage Review

Most people use a fraction of OpenClaw, and upgrades add features nobody notices. This skill reads real usage records, suggests "try it this way" with evidence, and after an upgrade maps the newly available features to how the person actually works. A settings doc acts as the filter (operating principles and a do-not-repropose list).

## Ground rules

- Read and suggest only. Config changes, upgrades, restarts, and session archiving need the owner's approval.
- With multiple agents, pass `--agent <id>` or `--all-agents` to the CLI.
- Never dump the whole config; read single paths with `openclaw config get <path>`. Never print or record token/key values.
- Confirm every suggested feature exists in the installed docs (`$(npm root -g)/openclaw/docs`, `openclaw docs <query>`). Label anything unconfirmed "assumed".
- Base session stats on `lastInteractionAt`, not `updatedAt` (migrations bulk-update `updatedAt`).

## A. Run a usage review now

1. Gather usage: run the "1. Usage" commands from [references/weekly-review-prompt.md](references/weekly-review-prompt.md) in the conversation.
   Done when: you have numbers for usage per agent and surface (dashboard / mobile / channel), long chats, subagents, and skill and automation use.
2. Match the data against [references/usage-signals.md](references/usage-signals.md), pick candidates, and confirm each feature and its usage in the docs.
   Done when: every candidate has a docs path or an "assumed" label.
3. Drop do-not-repropose items from the settings doc and keep at most 5. Format: observation (data) → try it this way → how to start (command or UI path) → expected benefit → caveats ("needs approval" if a config change is involved).
   Done when: all five fields are filled for every suggestion.

## B. What's new after an upgrade

1. Establish the previous version (from records or by asking) and `openclaw --version`, then read every release note in between (https://github.com/openclaw/openclaw/releases).
   Done when: notes for every intermediate version are read.
2. Keep only new features that overlap the usage patterns from A-1 and write them as "try it this way". List unrelated changes as one-liners.
   Done when: each new feature names the usage pattern it connects to.
3. Check the upgrade didn't break anything with [references/post-upgrade-checklist.md](references/post-upgrade-checklist.md).
   Done when: every checklist row has a result.

## C. Set up the weekly usage review automation

1. If no settings doc exists, create one from [references/settings-doc-template.md](references/settings-doc-template.md) first — its principles and do-not-repropose list filter the suggestions.
   Done when: the doc has a do-not-repropose list.
2. Fill every `{{…}}` in [references/weekly-review-prompt.md](references/weekly-review-prompt.md).
   Done when: no `{{` remains.
3. Register it under the ops agent:
   `openclaw cron add --agent <ops-agent> --name openclaw-weekly-review --cron "0 10 * * 6" --tz <IANA tz> --exact --session isolated --timeout-seconds 1800 --announce --channel <channel> --account <account> --to <target> --message "$(cat <prompt-file>)"`
   Then `openclaw cron edit <id> --clear-tools` — the default isolated tool list lacks exec/file tools, and CLI backends such as Claude CLI cannot enforce a restricted list.
   Done when: `openclaw cron get <id>` shows the schedule, delivery, and `toolsAllow: ["*"]`.
4. Test run with `openclaw cron run <id> --wait --wait-timeout 9m`, then recount the evidence behind each suggestion against live data to catch false positives (counts including archived sessions, conflicts with owner decisions) and fix the prompt or the do-not-repropose list.
   Done when: the report and state files exist, `cron runs` shows `delivered: true`, and every false positive has a fix location.

## D. Right after a settings change

In the same turn, update the settings doc's current settings and change log; add proposals the owner rejected to do-not-repropose with a date.
Done when: `openclaw config get` for the changed path matches the doc.

## Verification

- Recount the evidence for 2 random suggestions; the numbers match the report.
- Every suggested feature's docs path exists.
- The summary sent to the owner includes the report (or doc) path.
