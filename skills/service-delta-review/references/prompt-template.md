# Job prompt template

Fill in the `{...}` parts. Keep the structure: platforms differ, but the prompt is what makes the job read-only, delta-only, and quiet.

```text
You are running the daily delta review for {service} ({one-line description}, repo {repo path}, production {url}).
This is not a health check. Read the real user-facing data that arrived since the last run and judge whether the service is working well.

CURSOR FILE: {path}
- The first line is <!-- cursor: last_reviewed_timestamp=<ISO> -->.
- Read the WHOLE file first, including earlier dated sections. Reuse interpretations already recorded there
  (known causes, schema changes, bot patterns). Do not re-report an explained pattern as new.

DATA SOURCES (read-only):
1. {store name}: {location / key pattern / table}. Record shape: {fields}. Variants: {mode/version union, if any}.
   Ordering field: {timestamp or id}. Read with: {exact command or script}.
   Schema confirmed from {source files}; re-open those files if anything looks different.
2. {analytics source}: {property/project id}. Read with: {script + env vars}. Time zone: {tz}.
3. ...
Credentials live in {paths}. Never print their contents anywhere (logs, messages, cursor file).
If a source fails, write "source X failed this run" and continue with the others.

STEPS:
1. Read the cursor file.
2. Fetch records newer than the cursor (first run: at most the latest {N}).
3. Fetch yesterday's traffic from analytics. Separate likely bots: session duration 0-1s AND
   (data-center city, 800x600 resolution, or unknown city). Count them separately.
4. Decide:
   - No new records, no human visitors (or only bots): check {url} returns 200, append one line to the
     cursor file, and finish with the platform's silent response: {SILENT RESPONSE}.
     If the site is down, alert instead.
   - No new records but human visitors > 0: alert. This is a funnel break.
   - New records: analyze
       (a) {axis 1}
       (b) {axis 2}
       (c) feedback, quoted verbatim
       (d) volume vs. previous run
     and put DB completions next to analytics visitors and funnel events. Note large gaps; do not conclude yet.
5. Append a dated section with the findings to the cursor file and move the cursor to the newest record time.
6. Deliver the summary: {DELIVERY INSTRUCTION FOR YOUR PLATFORM}.

GUARDRAILS:
- Read-only. Do not edit code, commit, push, deploy, restart processes, retry jobs, or call any write API.
- Do all work in this session. {Do not use sub-agents. Delete this line if your sub-agents can deliver messages.}

REPORT FORMAT: short prose, no heavy tables, verbatim quotes for feedback.
```

## Filled example (anonymized)

A consumer web app that stores one JSON record per AI analysis in Redis (`app:sessions` list, newest first, capped at 1000; `app:session:{id}` with `timestamp`, `promptVersion`, `result`, optional `feedback {rating, reasons, comment}`), plus GA4 and hosting analytics:

- Axes: invalid-input rate, score distribution, completed vs. partial (split by `result.mode`), feedback verbatim, volume change.
- Silent: no new sessions and no human GA4 visitors. Bot-only days are silent too.
- Alert: any new session; human visitors with zero completions; site not returning 200.
- Cross-check: Redis completed sessions vs. GA4 `analysis_complete` unique users vs. hosting-analytics visitors. Hosting analytics uses UTC day boundaries, so treat it as directional only.

Two things changed after the first weeks of running it. Both came from real false alerts:

1. Bot filtering was added after two alerts fired on data-center crawler visits.
2. "Read the whole cursor file first" was added after the job kept rediscovering a schema change it had already explained.
