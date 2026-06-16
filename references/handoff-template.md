# Handoff Template

## Format Rules

- Compressed notation only — never prose
- Tables > bullet lists > prose (preference order)
- Every field: minimum tokens to reconstruct full context
- Status icons: ✓ done / ⏳ in progress / ❌ abandoned / 🔗 referenced / ⚠️ needs validation

---

# HANDOFF — {SLUG} — {YYYY-MM-DD HH:MM}

## BRIEF
{2 sentences max. What we were doing + why. No more.}

## GOAL STACK
| Level | Description |
|-------|-------------|
| Surface | {The immediate task being executed} |
| Actual | {The real outcome the user needs — often different from the surface task} |

## LAST CURSOR ← MOST IMPORTANT FIELD — READ THIS FIRST
```
{Be surgical. Not a summary — the exact state at moment of handoff.}

{Good example:}
Mid-row 3 of the Farmley pricing table (slide 6). Need to find Farmley 100g 
SKU price before filling this cell. Once done: complete rows 4–5, then add 
footnote on data source. The slide is otherwise complete.

{Bad example:}
Working on the pricing slide.
```

## DECISIONS
| Decision | Rationale | Reversible? |
|----------|-----------|-------------|
| {what was decided} | {why — the reasoning that should survive this session} | Y / N |

## FAILED ATTEMPTS
| Tried | Why Abandoned |
|-------|---------------|
| {approach or idea} | {specific reason it didn't work — be precise so it's not tried again} |

## CONSTRAINTS
**Hard** (non-negotiable — do not override):
- {constraint discovered or stated this session}

**Soft** (preferences — follow unless user directs otherwise):
- {preference}

## OPEN THREADS
{Ordered by priority. Top item = first action in next session.}
- [ ] {action or question}
- [ ] {action or question}

## ARTIFACT GRAPH
| Item | Path / URL | Status |
|------|-----------|--------|
| {name of file, doc, design, script} | {exact path or URL} | ✓ / ⏳ / ❌ / 🔗 |

## ENVIRONMENT
| Field | Value |
|-------|-------|
| Platform | {Claude.ai / Cowork / Claude Code} |
| Branch | {git branch if applicable} |
| Active MCP | {MCP server names in use} |
| Key handles | {design IDs, file IDs, API key names — not values} |
| Skills active | {skill names loaded this session} |

## VOCABULARY
| Term Used | Refers To |
|-----------|-----------|
| "{shorthand}" | {what it actually means in this context} |

## ASSUMPTIONS
{Things operated on as true but never explicitly confirmed. Flag for re-validation.}
- ⚠️ {assumption}

## URGENCY
{Deadline or pressure affecting priority. Write "none" if clear.}
- {deadline or context}

## META
{1 sentence. Why this work matters — what's at stake or what depends on it.}

---
*Created: {timestamp} | Resume: paste this doc and say "load handoff" or "load handoff {slug}"*
