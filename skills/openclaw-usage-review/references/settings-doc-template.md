# OpenClaw settings doc template

Copy and fill. Write "n/a" when a setting does not apply and "assumed: …" when the reason is unconfirmed.

```markdown
# OpenClaw Settings Definition

> Source of truth for "how this OpenClaw is set up, and why" on <host>.
> The weekly review automation compares live settings against this doc and proposes improvements.
> Update this doc whenever a setting changes.
>
> Last checked: YYYY-MM-DD · version <openclaw --version>

## Operating principles (owner decisions)
- Who approves config changes, upgrades, restarts — and how
- Upgrade policy (e.g. owner upgrades major versions; recommend only after changelog + regression check)
- Billing policy (e.g. subscriptions first; allowed pay-as-you-go exceptions)
- How uncertainty is labeled

## Current settings and reasons

### Gateway / service
- Service unit and restart command — reason
- External access path (tunnel, proxy, allowed origins) — reason
- Runtime options such as memory limits — reason

### Models
- Primary / utility / fallback model — reason
- Picker allowlist and aliases — reason
- Per-provider auth order (are pay-as-you-go paths blocked?) — reason

### Auth
- Expiry cycle, re-auth steps, and monitoring job for each login type

### Memory
- Engine, embedding provider, model, key location — reason (including billing)
- Indexed paths; extras such as dreaming on/off — reason

### Workspace files
- Where bootstrap files (SOUL/IDENTITY/USER/MEMORY) and the memory folder physically live, and how they sync with an external notes store — reason

### Automations
- Name · schedule · execution (main/isolated, subagents or not) · delivery — reason

### Channels
- Channels, accounts, bindings, display options — reason

### Skills / sessions
- Skill learning mode, session grouping and archive policy — reason

### Security / secrets
- How secrets are stored (SecretRef targets)
- Accepted residual warnings (item + date)
- External integrations (MCP etc.) and where their tokens live

## Do not repropose (only with evidence that conditions changed)
- Item — decision date — one-line reason

## Change log
- YYYY-MM-DD: what changed and why, one line
```
