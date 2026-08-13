# common-ux.md structure and update guide

For evidence-status notation (`[confirmed]` / `[confirmed: absent]` / `[needs confirmation]` / `[contradiction]`), see "Evidence-status notation" in SKILL.md. Common rules use the same notation as domain documents — this distinction matters especially between "this service simply doesn't have this pattern" (`[confirmed: absent]`) and "not decided yet" (`[needs confirmation]`), so domain documents linking to this file don't misread it.

## File structure

```markdown
# Common UX Definitions

## 0. UX Principles
## 1. Common component behavior (modals / inputs / buttons, etc.)
## 2. Time formatting
## 3. Number / count formatting
## 4. Images / thumbnails
## 5. Logged-in / logged-out branching
## 6. Empty states
## 7. Toast messages
## 8. Interaction patterns (global patterns not tied to a component: scroll-based behavior, etc.)
## 9. Client branching criteria
## 10. Global service exceptions
```

## Update principle

When a new common pattern shows up while writing a domain document, **add it to common-ux.md immediately** and reference it by link from the domain document.

```markdown
# How to reference it from a domain document
| Time formatting | → [common-ux.md > Time formatting] |

# When there's an exception
| Time formatting            | → [common-ux.md > Time formatting]         |
| Time formatting, exception | Ranking screen: shows date only past 24h   |
```

## Criteria for writing each section

### 0. UX Principles

- Summarize the higher-level values/philosophy this service's UX follows in 3-5 short statements. These aren't concrete rules but **judgment criteria** — use them to resolve ambiguous cases in every other section and in each domain document.
- These are usually derived from the product's value proposition (Business Discovery, etc.) or from patterns already observed repeatedly in code/domain documents. Don't invent principles that aren't grounded — cite the observed evidence next to each one.
- Because this isn't directly verifiable from code but sits in product philosophy/decision territory, always get sign-off from the document owner after drafting — don't finalize it unilaterally.

### 1. Common component behavior (modals / inputs / buttons, etc.)

- **Boundary rule**: "behavior this component has identically no matter where it's used" belongs here; "which fields/values appear in this component on this particular screen" belongs in the domain document. Example: "does the modal close on ESC" is common; "which input fields does this specific modal have" is domain-specific.
- Give each component its own subheading (e.g. 1.1 Modal, 1.2 Input, 1.3 Button). Only add components that actually exist in the service — don't pre-create entries for components that aren't used.
- If the same kind of field (e.g. name/description) shows up repeatedly across multiple domain documents and it's unclear whether the constraints (character limits, etc.) are actually shared, only promote it here after cross-checking the code confirms it's genuinely a common convention — until confirmed, leave it recorded individually in each domain document.

### 2. Time formatting
- Relative-time thresholds: just now, N minutes ago, N hours ago, N days ago, etc.
- The cutoff where it switches to absolute time (e.g. YYYY.MM.DD after 24 hours)
- Organize per-client differences into a table if they exist

### 3. Number / count formatting
- K/M truncation thresholds (e.g. 1,000 → 1K)
- How decimals are handled
- Price/percentage formatting

### 4. Images / thumbnails
- Fallback-image handling
- How the loading state is shown
- Aspect-ratio rules

### 5. Logged-in / logged-out branching
- Which features are accessible vs. blocked when logged out
- When the login prompt triggers
- Whether the user returns to the original screen after logging in

### 6. Empty states
- Distinguish three cases: no data / error / loading
- What text each state shows
- Whether a CTA button is included

### 7. Toast messages
- Display duration (default)
- Position (top/bottom)
- Success/failure/info type distinctions

### 8. Interaction patterns (global patterns not tied to a component)
- Scroll-based behavior (infinite scroll, pagination cutover criteria)
- Pull-to-refresh criteria
- Only patterns that are ambiguous to place under "1. Common component behavior" and aren't tied to a specific component go here

### 9. Client branching criteria
- Web / App / mWeb distinction criteria
- A table of per-feature client support

### 10. Global service exceptions
- Per-language (KO/EN/JA) notation differences
- Per-region date/number format differences
