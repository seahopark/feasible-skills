# service-delta-review

## Goal

Set up a scheduled, read-only job that reads only the user data that arrived since the last run (sessions, AI outputs, feedback, analytics), checks whether the service is working well for users, and sends a message only when something needs attention.

## When to use

1. A live service stores user-facing results or feedback, and you want an agent to review the new ones every day.
2. You want daily monitoring without "nothing changed" messages on quiet days.
3. You have product data (DB, Redis) and traffic data (GA4, PostHog) and want them cross-checked: visitors who never finish, tracking gaps, bot traffic.
4. You run Hermes, OpenClaw, Claude Code, or plain cron and need install steps for the job.

## Files

- [`SKILL.md`](SKILL.md): the full procedure the agent follows
- `references/`: [`platforms.md`](references/platforms.md), [`prompt-template.md`](references/prompt-template.md)
