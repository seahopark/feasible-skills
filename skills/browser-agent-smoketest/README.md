# browser-agent-smoketest

## Goal

Write a full-cycle smoke test as a prompt that an AI browser agent (e.g. Claude in Chrome) can run by itself, including the rules it needs to execute safely: wait policy, file uploads, side-effect guards, cleanup, result logging.

## When to use

1. Right after a deploy, you want a quick check that the core flows still work.
2. You want an AI to run the smoke test instead of a person.
3. You got results from a previous run and want to fold them into the next version of the test.

## Files

- [`SKILL.md`](SKILL.md): the full procedure the agent follows
- `references/`: [`template-and-example.md`](references/template-and-example.md)
