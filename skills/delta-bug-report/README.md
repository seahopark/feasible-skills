# delta-bug-report

## Goal

Read the code changes between two commits and find client-side logic bugs. Each bug gets a one-line summary for deciding whether to fix it, plus English technical detail (file:line, cause, repro, fix direction) to paste into an IDE coding agent.

## When to use

1. You pulled the latest code and want to check it for bugs before deploying.
2. You need a bug report to hand to engineers.
3. After a release or migration, you want to find what broke.

## Files

- [`SKILL.md`](SKILL.md): the full procedure the agent follows
- `references/`: [`output-format-example.md`](references/output-format-example.md), [`subagent-prompt-template.md`](references/subagent-prompt-template.md)
