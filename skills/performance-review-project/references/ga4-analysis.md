# Analytics review

Use this reference when the selected source is GA4 or another event-based product analytics
system. It supports an independent analytics review; Linear or another work-record source is not
required.

## Review sequence

1. Confirm the property, timezone, date range, population, and internal-traffic or other filters.
2. Confirm that the metric and event definitions in `metrics.md` match the source.
3. Check that the events and parameters exist in the selected period. Do not infer collection
   from application code alone when the analytics system or tag manager is the source of truth.
4. For a funnel, record the ordered events, the denominator for every conversion rate, and
   whether the count is events, sessions, or users.
5. Run a small query first, then the full query.
6. Save the query or a reproducible description, retrieval date, and result status in the report.

## Result status

Use separate labels:

- **access failure:** the agent or credential could not reach the source;
- **query failure:** the source was reachable but the request was invalid or failed;
- **no data:** the query was valid and returned no records;
- **not instrumented:** the event or parameter was not available in the source;
- **observed zero:** the valid query returned a measured zero;
- **unverified:** the value cannot be reproduced from the recorded conditions.

Do not interpret a zero attributed source or campaign as proof that there was no traffic. Check
attribution gaps, direct traffic, in-app browsers, and an independent source when the conclusion
matters.

## Minimum metric record

```text
Metric or event:
Meaning:
Population / denominator:
Date range:
Timezone:
Dimensions and filters:
Aggregation: event count / sessions / users / rate / cohort
Source and property:
Retrieved at:
Status:
```

For a funnel, add the previous and next step and the expected change from the project hypothesis.
