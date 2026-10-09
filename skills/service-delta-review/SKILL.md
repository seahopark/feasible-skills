---
name: service-delta-review
description: |
  Design and install a recurring, read-only "delta review" job for a live service:
  each run reads only the data that arrived since the last run (sessions, AI outputs,
  user feedback, analytics), extracts quality signals, and stays silent unless something
  is actually worth a human's attention. Not a health check — it asks "is the service
  working well for users?", not "is it up?".

  Use when:
  - "Set up a daily job that reviews this service's data and tells me what to improve"
  - "Make a monitoring / daily review cron, but don't spam me when nothing happened"
  - A service already stores user-facing results or feedback, and you want someone
    (an agent) to read the new ones every day
  - You want "only what changed since last time" instead of re-scanning everything
---

# Service Delta Review Skill

The job is an observer. It reads new data, compares it with what it already knows, and reports only exceptions. It never changes the system it observes.

"Delta" here means the user data that arrived since the last run, not code changes. To review a git diff for bugs, use `delta-bug-report` instead.

## Core principles

- **Quality review, not uptime.** The target must persist something that reflects quality: AI responses, scores or verdicts, task outcomes, user feedback, funnel events. If nothing like that is stored, you need a health check, not this skill.
- **Read-only, always.** No code edits, commits, deploys, restarts, retries, or write calls to the data store. Put this in the job prompt in so many words. The job reports; humans act.
- **Delta only.** A cursor file records the last reviewed timestamp or record ID. Each run fetches records after the cursor, analyzes them, then advances the cursor. Cost stays flat as data grows, and attention goes to what is new.
- **The cursor file is also the job's memory.** Each run appends a dated section with findings and interpretations (schema changes, known causes, "this spike was a bot"). The next run reads the whole file first and inherits those interpretations, so it does not re-report an explained pattern as a new discovery.
- **Silent by default.** No new data, or metrics inside normal range → write one line to the cursor file and end without notifying anyone. Low-traffic services otherwise produce daily "nothing changed" noise and the alerts stop being read.
- **The diagnosis decides whether to alert, not the scheduler.** Use the platform's "suppress delivery" mechanism (see `references/platforms.md`) so the job itself chooses to speak. Keep the platform's failure alert on: "silent because nothing happened" and "silent because the job crashed" must look different.
- **3–5 analysis axes per service.** Pick the metrics that best separate "working" from "not working" for this product (e.g. failure or invalid rate, score distribution, completion vs. drop-off, feedback). Quote qualitative feedback verbatim; summaries lose the nuance that matters.
- **Cross-check sources when you have more than one.** Product data (your DB) and traffic data (analytics) disagree in useful ways: visitors with zero completions is a funnel signal; analytics far below DB counts suggests consent-blocked tracking. Report the comparison; do not force an interpretation until a few weeks of data exist.
- **Filter non-human traffic before alerting.** Bot visits (0–1s sessions from data-center cities, 800x600 resolution, unknown city) can trigger false alerts. Count them separately and do not alert on bot-only traffic.
- **Each source can fail independently.** If one source errors (expired token, rate limit), record "source X failed this run" and continue with the rest. One flaky API must not kill the whole review.
- **Credentials stay invisible.** Read-only scopes only. Never print keys or tokens to logs, messages, or the cursor file.

## Workflow

### 1. Detect what data exists

Read the service's code before writing any prompt. Find where results are persisted:

- Search for write paths: logger/analytics modules, DB inserts or ORM `create`/`save`, `LPUSH`/`XADD`/`SET` to Redis, event `track()` calls, files written per request.
- For each store, record: location (table, key pattern, log path), record shape (fields, types, union/versioned variants), ordering and retention (e.g. list capped at 1000, TTL), and the timestamp or ID field usable as a cursor.
- Note external sources already wired up: product analytics (GA4, PostHog, Amplitude), hosting analytics, error trackers.
- Check how the job can read each one (API, read replica, read-only credentials) and confirm it can do so **without** write permission.

Done when: you can name the unit of "one new thing" (a session, a response, a feedback entry) and the field that orders it. If you cannot, stop. A job built on a guessed schema breaks silently on the first schema change.

### 2. Decide the cursor location

Pick one file the job reads and writes every run, in a place a human can also read (workspace memory folder, notes vault, repo-ignored path). First line holds the cursor, e.g. `<!-- cursor: last_reviewed_timestamp=2026-10-09T00:00:00Z -->`. Seed it once with a recent past time so the first run has a bounded window.

### 3. Choose the analysis axes

From the data found in step 1, choose 3–5 axes. Typical set:

1. Outcome quality: failure / invalid / fallback rate, score or verdict distribution
2. Completion: finished vs. partial or abandoned, split by version or mode if the schema has variants
3. Feedback: ratings plus verbatim comments
4. Volume: change vs. previous run
5. Traffic cross-check (optional): analytics visitors and funnel events next to DB completions, bots excluded

### 4. Define silence and alert conditions

Write both explicitly. Example:

- **Silent:** zero new records and zero human visitors; or only bot traffic; or all metrics within the range noted in the cursor file. Optionally run a cheap liveness check (HTTP 200 on the main URL) and alert only if it fails.
- **Alert:** any new records (send the summary); human visitors but zero completions (funnel break); failure rate jump; unusual feedback; service down.

Tune the alert bar to the traffic level. At low volume "any new real usage" is itself worth a message; at high volume alert only on deviations.

### 5. Write the job prompt and install it

Use `references/prompt-template.md`. The prompt must contain:

- (a) cursor file path, and an instruction to read the whole file first and inherit prior interpretations
- (b) data sources with exact read commands or scripts, and the confirmed schema (with "re-check these source files if in doubt")
- (c) the analysis axes from step 3
- (d) the silence and alert rules from step 4, including the exact silent response for your platform
- (e) the guardrail: read-only, no code changes, commits, deploys, retries, or write calls; never print credentials
- (f) "do the work in this session; do not delegate to sub-agents" if your platform's sub-agents cannot send messages
- (g) final report format: short prose, verbatim quotes for feedback

Install on your scheduler (`references/platforms.md` covers OpenClaw, Hermes, Claude Code, and plain cron). Schedule after the data for a period has settled, e.g. 01:00 local time for "yesterday".

### 6. Verify with forced runs

Run the job manually once or twice:

- With new data: does it produce a useful signal, and does it advance the cursor?
- Without new data: does it stay silent and add one line to the cursor file?
- Break one source on purpose (wrong token): does it record the failure and continue?

Expect to adjust the axes after seeing real output. That is normal; most of the tuning happens here.

## Notes

- If the service already has a report-style job (a daily digest), send this job's alerts to a different channel so observations and routine reports do not mix.
- If other people or agents actively work on part of the service (ads, tracking setup), exclude that area from the job's scope explicitly so the observer does not mix into their work.
- When a data source changes schema, the job should record the change in the cursor file the first time it sees it. Future runs then handle both shapes.
