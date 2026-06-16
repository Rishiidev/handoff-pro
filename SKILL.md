---
name: handoff-pro
description: >
  Eliminates starting-from-zero when context fills up or a session ends. Two
  modes — SAVE compresses the current session into a structured handoff document;
  LOAD reads a previous handoff and resumes with full context as if the
  conversation never ended. Invoke immediately when the user says any of:
  "handoff", "create handoff", "save handoff", "load handoff", "load latest
  handoff", "resume", "load session", "save session", "pick up where we left
  off", "continue from last session", or when the context window is visibly
  filling up. In Cowork and Claude Code, loading is fully automatic — no
  copy-paste needed. Works across Claude.ai, Cowork, and Claude Code.
---

# Handoff Pro

Preserves full session context between conversations so the next session feels like a continuation — not a restart. The previous session's decisions, constraints, vocabulary, failed attempts, and exact stopping point are all restored before any new work begins.

**Two modes:**
- `SAVE` → compress current session → write or display handoff doc
- `LOAD` → read handoff doc → internalize context → brief user → resume

---

## Environment Detection

Before either mode, detect the environment once and adapt I/O accordingly.

| Signal | Environment |
|--------|-------------|
| Filesystem + project folder accessible | Cowork |
| `bash_tool` available | Claude Code |
| No filesystem access | Claude.ai web |

**Storage paths:**
- **Cowork**: `{project_root}/.handoffs/` (preferred) or `~/.claude/handoffs/` (global fallback)
- **Claude Code**: `.claude/handoffs/` in project root, or `~/.claude/handoffs/` globally
- **Claude.ai**: Artifact only — user copies and pastes into next session

Filenames: `YYYY-MM-DD-HHMMSS-{slug}.md`
Slug = 2–3 word session summary, auto-generated (e.g. `bunchshop-pricing-deck`, `ghosts-pwa-auth`)

---

## SAVE Mode

**Triggered by:** "handoff", "create handoff", "save handoff", "save", or context window running visibly long.

### Step 1 — Extract the 12 Elements

Read the full conversation and extract every field below. Do not skip or guess — missing fields are exactly why handoffs feel incomplete in the next session.

```
1.  GOAL STACK        The surface task + the actual underlying outcome the user needs
2.  LAST CURSOR       The exact thought / line / file / step at moment of handoff
3.  DECISIONS         Every non-trivial decision made, with rationale
4.  FAILED ATTEMPTS   What was tried and abandoned — prevents re-running dead ends
5.  CONSTRAINTS       Hard (non-negotiable) + soft (preferences discovered mid-session)
6.  OPEN THREADS      Things noted but not yet addressed, ordered by priority
7.  ARTIFACT GRAPH    Every file, doc, design, URL, code in play — with status
8.  ENVIRONMENT       Active branch / MCP / tool / design ID / file handles / skills
9.  VOCABULARY        Shorthand, nicknames, and abbreviations used this session
10. ASSUMPTIONS       Things operated on as true but never explicitly confirmed
11. URGENCY           Deadlines or pressure that affects priority order
12. META CONTEXT      Why this work matters — what's at stake
```

**Critical:** LAST CURSOR (#2) is the most important field. It is not a summary — it is the exact thought mid-action. Example of wrong vs right:

```
WRONG:  "We were working on the pricing slide."
RIGHT:  "Mid-row 3 of the pricing table, Farmley 100g column.
         Next action: find Farmley 100g SKU price, fill cell,
         then complete rows 4-5 before adding footnote."
```

### Step 2 — Generate the Handoff Document

Use compressed format — tables and structured fields, not prose. See `references/handoff-template.md` for the exact document structure.

Rule: every field should carry the minimum tokens needed to reconstruct full context. If you find yourself writing paragraphs, compress them.

### Step 3 — Save or Display

**Cowork / Claude Code:**
1. Create handoffs directory if it doesn't exist
2. Write file: `{handoffs_dir}/YYYY-MM-DD-HHMMSS-{slug}.md`
3. Update index: append a new row to `{handoffs_dir}/index.md`
4. Confirm path to user

**Claude.ai:**
1. Generate the document as a markdown artifact
2. Add this note at the bottom of the artifact:
   > *Save this document to Notion, Apple Notes, or any file. To resume: paste it at the start of your next session and say "load handoff".*

### Step 4 — Confirm Save

Report back concisely:
- File path (or "saved as artifact")
- Slug
- The single most critical next action from this session

---

## LOAD Mode

**Triggered by:** "load handoff", "load latest handoff", "resume", "load", "pick up where we left off", "continue from last session", or when a handoff document is pasted at the start of a conversation.

### Step 1 — Find the Handoff

**Cowork / Claude Code — fully automatic, zero prompting:**

Any load trigger without a specific slug = find and read the most recent handoff immediately. Do not ask. Do not show a list. Just load it.

```
Priority order:
1. {project_root}/.handoffs/   ← check here first (project-scoped)
2. ~/.claude/handoffs/         ← fall back to global

Within the folder: sort files by timestamp prefix (YYYY-MM-DD-HHMMSS-*)
→ pick the latest, read it, proceed.
```

Only show the index list when the user explicitly asks — e.g. "show my handoffs" or "load handoff [specific-slug]".

**Claude.ai:** The handoff document is pasted directly in the conversation. Read it from there.

### Step 2 — Parse and Internalize

Read all 12 fields silently. Do not output anything yet.

Flag staleness before resuming:
- Doc older than 7 days → warn the user
- Referenced files/URLs → check existence if filesystem is accessible
- Assumptions → mark for re-validation if context has likely shifted

### Step 3 — Brief the User

Output one compact brief — not a dump of the entire document:

```
RESUMED: {slug} — {date}
────────────────────────────────────────
WHERE WE LEFT OFF:
{LAST CURSOR field — 2-3 sentences max}

FIRST ACTION:
{Top item from OPEN THREADS or the next logical step}

WATCHOUTS:
{Stale items, unvalidated assumptions, or blockers}
────────────────────────────────────────
Ready. Say "go" to continue or give me a new direction.
```

### Step 4 — Resume

Work as if the conversation never ended.

- Apply VOCABULARY silently — do not re-explain what terms mean, just use them
- Apply CONSTRAINTS silently — they are now operating rules, not things to announce
- Apply ASSUMPTIONS — treat them as working truth unless the user corrects them
- Respect DECISIONS — do not re-open settled choices without a reason from the user

Do not narrate the handoff back. The user already knows the context. You are the one who needed the briefing. Act like you were there.

---

## Handoff Index

Maintained at `{handoffs_dir}/index.md`. Never delete entries — history is the point.

```markdown
# Handoff Index

| # | Date | Slug | Last Cursor (brief) | Status |
|---|------|------|---------------------|--------|
| 001 | 2025-01-15 14:32 | bunchshop-pricing-deck | Mid slide 6 pricing row, need Farmley 100g price | ✓ resumed |
| 002 | 2025-01-16 09:10 | ghosts-pwa-dark-theme | localStorage snapshot bug, line 142 | ⏳ active |
| 003 | 2025-01-20 11:05 | api-rate-limiter | Drafted Redis approach, rejected — see FAILED | 📦 archived |
```

Status values: `⏳ active` → `✓ resumed` → `📦 archived`

Mark as `✓ resumed` when loaded. Mark as `📦 archived` when the work is fully complete or superseded.

---

## Proactive Triggering

If the conversation is long and no handoff has been saved, suggest it naturally:

> "This session has covered a lot of ground. Want me to create a handoff before we go further? Just say 'create handoff' — takes 30 seconds."

Suggest this after ~25 back-and-forth exchanges or when you detect context pressure. Do not wait for the user to notice.

---

## Reference Files

- `references/handoff-template.md` — exact document template with field-by-field guidance
