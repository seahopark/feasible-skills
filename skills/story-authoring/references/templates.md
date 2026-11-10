# Story Templates by Type

Do not fill blanks with plausible content. Use `TBD` or `needs confirmation` when evidence is
missing.

## Feature / Design

````markdown
**Why:**
[Why this work is needed now: problem, context, and evidence]

**As a** [specific user or role]
**I want** [the goal they need to reach]
**So that** [the value gained by reaching it]

### Acceptance Criteria

```gherkin
Scenario: [user situation being verified]
Given [required starting conditions]
When [user action or event]
Then [observable result]
```

**Notes:**
- [constraint, evidence, or related story]
- [TBD or fact that needs confirmation]
- [edge cases deliberately excluded; ownership boundary with related stories; shared criteria]
````

When there are multiple Scenarios, make each one independently readable. Put an implementation
approach in Notes only when it is a real technical constraint.

## Bug

```markdown
### Environment

[device/OS/app version, account permissions, data conditions]

### Steps to reproduce

1. [observed step]
2. [observed step]

### Reproduction rate

[always / intermittent — approximate frequency]

### Expected

[expected behavior]

### Actual

[actual behavior, error message, console log, or screenshot]

### Notes

- [condition, log, screen, or other evidence]
- [anything not reproduced or still needing confirmation]
- [reason for splitting out failures with a different cause]
```

Use only observed steps. Do not put an unverified cause in Actual.

## Chore

```markdown
**Context:**
[why this internal or operational work is needed]

**Work:**
- [work in scope]

**Done when:**
- [observable completion condition]

**Notes:**
- [constraint, dependency, or unknown]
```

Use plain Markdown. Do not add Gherkin or a forced user persona unless explicitly requested.

## Release

```markdown
**Release goal:**
[purpose and the change delivered to users or the market]

**Included:**
- [included story or change]

**Release checks:**
- [deployment, operations, or communication check]

**Notes:**
- [constraint, dependency, rollback condition, or unknown]
```

Do not copy every implementation requirement into the Release. Link included stories and state only
release-level checks.
