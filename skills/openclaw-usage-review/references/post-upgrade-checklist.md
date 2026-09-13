# Post-upgrade checklist

Run top to bottom. "What to confirm" is judged on the artifact, not on a command saying ok.

| # | Area | Command | What to confirm | Common trap |
|---|---|---|---|---|
| 1 | Version & service | `openclaw --version`, `systemctl --user status openclaw-gateway.service` | Expected version, active, restart command matches the doc | The service moved from system to user scope, so the old `sudo systemctl restart …` fails with "Unit not found" |
| 2 | Bootstrap files | `journalctl --user -u openclaw-gateway.service --since "<upgrade time>" \| grep "bootstrap file is unreadable"` | Zero matches | Symlinks pointing outside the workspace get rejected, so the agent runs without SOUL/MEMORY. Use real files and mirror them to the external store |
| 3 | Memory writes | Today's daily note exists; `journalctl … \| grep -i "Refusing to append"` | File really exists; zero refusals | The checkpoint job replies "saved" but no file was written |
| 4 | Heartbeat | Find the heartbeat job in `openclaw cron list --all --json`, then `openclaw cron scratch <id>` | The checklist is in the scratch | The runtime no longer reads `HEARTBEAT.md`; `doctor --fix` migration only works from a shell outside the gateway |
| 5 | Memory search | `openclaw memory status --deep --agent <id>` | Provider/model match the doc, index complete, vector search not paused | A removed memory engine flips the default to a paid embedding provider, which silently reindexes with any leftover API key |
| 6 | Models & auth | `openclaw models status`, `openclaw models auth order get --provider <p> --agent <id>` | Primary/fallback/utility/allowlist and auth order match the doc | The fallback model can bill an API-key profile instead of a subscription login |
| 7 | Channels | `openclaw channels status` | Each account running and connected | A WARN in `openclaw status` can just mean the CLI cannot read a SecretRef — judge by gateway channel status. Duplicate tokens across accounts can become errors in newer versions |
| 8 | Automations | `openclaw cron list --all --json`, `openclaw tasks audit` | No failing jobs, no delivery_failed | Subagents may lack the message tool, so reports get lost silently; have the job do the work itself |
| 9 | Lint, secrets, security | `openclaw doctor --lint --json`, `openclaw secrets audit --check`, `openclaw security audit` | No new findings outside the doc's accepted list | With multiple agents, `secrets configure` fails without `--agent` |
| 10 | Session stats | `openclaw sessions --all-agents --json --limit all` | Counts use `lastInteractionAt` | Migrations bulk-update `updatedAt`, inflating "last 7 days" |

When done, show the list of differences to the owner, apply only approved changes, then update the doc's current settings and change log.
