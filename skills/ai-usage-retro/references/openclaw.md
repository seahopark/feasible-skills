# OpenClaw commands for the tool-side review

Follows [tool-side-review.md](tool-side-review.md). Same commands as `openclaw-usage-review`; its [usage signals](../../openclaw-usage-review/references/usage-signals.md) (signal → candidate → docs to confirm) and [post-upgrade checklist](../../openclaw-usage-review/references/post-upgrade-checklist.md) apply here too.

## Rules while running
- Read only. Never change config, upgrade, restart, change auth, delete files, or archive/delete sessions.
- With multiple agents, pass `--agent <id>` or `--all-agents`. Retry "SQLite source did not stabilize" up to twice after a short wait.
- Never dump the whole config; read single paths with `openclaw config get <path>`. Never print or record tokens or keys.
- Confirm each suggested feature in the installed docs (`$(npm root -g)/openclaw/docs`, or `openclaw docs <query>`); label anything unconfirmed "assumed".
- Base session stats on `lastInteractionAt`, not `updatedAt` (migrations bulk-update `updatedAt`).

## 1. Usage (last 7 days)
- `openclaw sessions --all-agents --json --limit all`: using `lastInteractionAt`, count sessions per agent and kind (dashboard / ios·android / channel / subagent / cron / harness), chats above 70% context, model mix, and sessions still carrying `modelOverride`. This output includes archived sessions; use the `sessions_list` tool (excludes archived by default) for an archive-free list.
- `sessions_list` per agent with `includeDerivedTitles`: titles and the share of chats with a group or label.
- For the 3–5 most active chats, read recent user messages with `sessions_history`.
- `openclaw skills list --json`, `openclaw skills workshop list`, `openclaw skills curator status --json`: skill use and pending proposals.
- `openclaw cron list --all --json`: automation results. `openclaw memory status --agent <id>` and date gaps in daily notes.
- `openclaw gateway usage-cost --all-agents --days 7`.

## 3. Versions
- `openclaw --version`, `npm view openclaw dist-tags --json`.
- Release notes: https://github.com/openclaw/openclaw/releases. For regression checks on a newer version, search that repository's open issues for the version string.

## 5. Breakage check
- `openclaw doctor --lint --json`, `openclaw tasks audit`, `openclaw channels status`, failing automations. Exclude tool-allowlist warnings that only appear on CLI stderr.
