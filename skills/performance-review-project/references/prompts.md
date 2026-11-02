# Review prompts

These prompts are deliberately written without vendor-specific tool names. In Claude Code,
save them under the project and optionally reference them from `CLAUDE.md`. Codex, OpenClaw, and
other agents can use the same files.

## First run: independent review

```text
You are reviewing one project. Read PROJECT.md, metrics.md, and sources.md first. Use the review
mode recorded in PROJECT.md. If only one source is available, review that source independently;
do not block the review waiting for the other source.

Do the following:
1. Restate the project problem and hypothesis in one paragraph. Ask about only facts that are
   necessary to interpret the review.
2. Identify the agreed metrics and their exact definitions, populations, periods, filters,
   aggregation, and timezone.
3. Read only the source needed for this review using read-only access. Record whether the source
   was reachable, the query succeeded, data was returned, and the relevant event or field existed.
4. Record the source conditions for every material number.
5. Separate observations, possible explanations, and unknowns. Include customer learning and
   failed or incomplete attempts when the records support them.
6. Write reports/YYYY-MM-DD.md with a conclusion first, followed by evidence, caveats, and next
   checks.

Do not choose substitute metrics without labeling them. Do not claim causation from a before/after
change alone. If a number cannot be reproduced, mark it unverified.
```

## Combined review

```text
Review this project using PROJECT.md, metrics.md, sources.md, and the optional source mapping.

First produce understandable independent sections for the work records and analytics. Only then
compare them using an explicit project/change identifier or a manually confirmed mapping. Record
the customer-facing release date and timezone, the analytics event or metric, the expected change,
the comparison windows, and competing changes.

Keep these separate:
- work completed;
- product or policy change;
- measured outcome;
- possible explanation;
- customer learning;
- what remains unknown.

Date overlap alone is not evidence of causation. If the sources cannot be mapped reliably, report
them independently and state the missing join key, release record, event, or comparison needed.
```

## Weekly review

```text
Review this project's latest period using PROJECT.md, metrics.md, sources.md, the previous report,
and the connected read-only source for the selected review mode.

Return:
- one-line conclusion;
- work completed or changed since the previous review;
- each agreed metric with current value, comparison value, period, filter, denominator, timezone,
  and source;
- meaningful changes only;
- competing explanations and data gaps;
- customer or user learning, including failed or incomplete attempts;
- decisions or checks for the next period.

Use the same metric definitions as the previous report. Do not silently change a period, filter,
population, or aggregation. If nothing meaningful changed, say so briefly and still record the
retrieval date and source status.
```

## Project retrospective

```text
Prepare a project retrospective from the project definition, work records, metric reports, and
decisions.

Organize it as:
1. What the project set out to change.
2. What was done and when.
3. What changed in the agreed metrics.
4. Which conclusions are supported, which are only hypotheses, and what cannot be attributed.
5. What should continue, change, or stop.

Keep evidence next to each material claim. Never turn an unverified number or a plausible cause
into a fact.
```
