# Installing the job on different schedulers

What each platform needs: run the prompt on a schedule in a fresh session, let the job suppress delivery when there is nothing to say, and alert when the job itself fails. Flags and field names change between versions, so check `--help` or the docs of the version you run.

## Hermes Agent

Hermes delivers the agent's final response to the `--deliver` target. To stay quiet, the agent replies with exactly `[SILENT]` and nothing else; Hermes then skips delivery. Do not tell the job to call a send-message tool. Hermes cron already adds a hint telling the agent to put its report in the final response.

```bash
hermes cron create "0 1 * * *" "$(cat delta-review-prompt.txt)" \
  --name "{service}-delta-review" \
  --deliver telegram \
  --workdir /path/to/service-repo \
  --profile {profile}
```

- `--workdir` loads that repo's `AGENTS.md` / `CLAUDE.md` and sets the working directory for file and terminal tools, which helps the job re-check schema files.
- In the prompt, set `{SILENT RESPONSE}` to `[SILENT]` and `{DELIVERY INSTRUCTION}` to "Write the summary as your final response."
- To keep a fetch step deterministic, put it in a script under `~/.hermes/scripts/` and attach it with `--script`. The script's stdout is injected into the prompt on every run.
- `--no-agent --script` is a different, LLM-free pattern (empty stdout = silent). Use it only for numeric thresholds. This skill needs the agent to read feedback and judge.
- Test: `hermes cron run <id>` (runs on the next scheduler tick), then `hermes cron list`.

## OpenClaw

```json
{
  "name": "{service}-delta-review",
  "schedule": { "kind": "cron", "expr": "0 1 * * *", "tz": "Asia/Seoul" },
  "sessionTarget": "isolated",
  "payload": { "kind": "agentTurn", "message": "<prompt>", "timeoutSeconds": 1200 },
  "delivery": { "mode": "none" },
  "failureAlert": { "mode": "announce", "channel": "telegram", "to": "<chat id>", "after": 1 },
  "enabled": true
}
```

- `delivery.mode: "none"`: the scheduler never auto-sends. The prompt tells the job to call the `message` tool only on the alert branch. On the silent branch it ends with a short final text and no message call.
- Keep `failureAlert`, so a crashed run is still visible.
- Tell the job not to use sub-agents: in some versions, spawned sub-agents had no `message` tool and their deliveries were lost.
- Leave `payload.toolsAllow` unset if you run on the `claude-cli` backend; setting it caused every run to fail in some versions.
- On CLI backends with a no-output watchdog, long silent tool chains can be cut off. Tell the job to write a one-line progress note after each data source.
- Test: `openclaw cron run <id>` (or the automations tool with `runMode: "force"`).

## Claude Code

- **Routines / `/schedule` (cloud):** create a scheduled agent with the prompt. The data sources must be reachable from the cloud environment, and the cursor file must live somewhere the routine can read and write (e.g. a repo branch or a notes repo). Delivery depends on the connectors you attach. Make the alert branch the only branch that posts.
- **Local cron + headless mode:** wrap `claude -p` in a script and send output only when it is non-empty:

```bash
#!/usr/bin/env bash
set -euo pipefail   # a failed agent run exits non-zero, so cron can alert on it
cd /path/to/service-repo
out=$(claude -p "$(cat ~/jobs/delta-review-prompt.txt)" --output-format text)
[ "$out" = "[SILENT]" ] || [ -z "$out" ] && exit 0
./notify.sh "$out"   # Slack webhook, Telegram bot, email...
```

  Set `{SILENT RESPONSE}` to `[SILENT]`. Restrict tools with your settings' permission rules so the job cannot write to the repo or call write APIs.

## Plain cron + any agent CLI

Same wrapper pattern as above: the agent prints `[SILENT]` or a report, the wrapper delivers only reports, and the wrapper's non-zero exit goes to cron's `MAILTO` or your alerting, so failures are not silent.
