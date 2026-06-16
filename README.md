# 🤝 Handoff Pro

**Never start from zero again.** Handoff Pro is a [Claude Skill](https://docs.claude.com/en/docs/claude-code/skills) that eliminates the "starting-from-zero" problem when your context window fills up or a session ends. It compresses an entire conversation into a structured handoff document — then reloads it later so the next session resumes as if it never stopped.

Works across **Claude.ai**, **Cowork**, and **Claude Code**. In Cowork and Claude Code, loading is fully automatic — no copy-pasting required.

---

## ✨ Why this exists

Every long Claude session eventually hits the same wall: the context window fills up, or the day ends, and you're forced to start a new conversation. By default, that means re-explaining:

- what you were actually trying to accomplish
- every decision you already made (and why)
- the dead ends you already tried (so you don't try them again)
- exactly where you left off, mid-thought

Handoff Pro captures all of that automatically, in a compact format, and restores it on demand.

---

## 🧠 How it works

Handoff Pro has two modes:

| Mode | What it does |
|------|---------------|
| `SAVE` | Compresses the current session into a structured handoff document and writes it to disk (or generates it as an artifact on Claude.ai) |
| `LOAD` | Finds the latest handoff, reads it silently, briefs you in a few lines, then resumes work as if the conversation never ended |

### The 12 elements every handoff captures

```
1.  GOAL STACK        The surface task + the actual underlying outcome you need
2.  LAST CURSOR       The exact thought / line / file / step at the moment of handoff
3.  DECISIONS         Every non-trivial decision made, with rationale
4.  FAILED ATTEMPTS   What was tried and abandoned — so it isn't retried
5.  CONSTRAINTS       Hard (non-negotiable) + soft (discovered preferences)
6.  OPEN THREADS      Things noted but not yet addressed, by priority
7.  ARTIFACT GRAPH    Every file, doc, design, URL, or piece of code in play
8.  ENVIRONMENT       Active branch / MCP servers / tools / design IDs / skills
9.  VOCABULARY        Shorthand, nicknames, and abbreviations used this session
10. ASSUMPTIONS       Things treated as true but never explicitly confirmed
11. URGENCY           Deadlines or pressure affecting priority order
12. META CONTEXT      Why the work matters — what's at stake
```

The most important field is **LAST CURSOR** — not a summary, but the *exact* in-progress state:

> ❌ "We were working on the pricing slide."
> ✅ "Mid-row 3 of the pricing table, Farmley 100g column. Next: find the Farmley 100g SKU price, fill the cell, then complete rows 4–5 before adding the footnote."

---

## 🚀 Installation

Drop this skill into your Claude Skills directory:

**Claude Code / Cowork:**
```bash
git clone https://github.com/Rishiidev/handoff-pro.git ~/.claude/skills/handoff-pro
```

**Manual install:**
1. Download or clone this repo.
2. Copy the folder into your skills directory (`~/.claude/skills/` or your project's `.claude/skills/`).
3. Restart your Claude Code / Cowork session — the skill is auto-detected via `SKILL.md`.

---

## 💬 Usage

### Saving a handoff

Just say any of:

```
"create handoff"
"save handoff"
"handoff"
"save session"
```

Claude will silently extract the 12 elements from the conversation, write a compressed markdown document, and confirm:

```
Saved: 2026-06-16-170300-bunchshop-pricing-deck.md
Next action: Find the Farmley 100g SKU price to finish row 3.
```

Claude also proactively offers to save a handoff once a session runs long (~25+ exchanges) — you don't have to remember to ask.

### Loading a handoff

Just say any of:

```
"load handoff"
"load latest handoff"
"resume"
"pick up where we left off"
"continue from last session"
```

**In Claude Code / Cowork**, this is fully automatic — Claude finds the most recent handoff file and loads it without asking. **On Claude.ai**, paste the saved handoff document at the start of your next conversation and say "load handoff."

You'll get a compact brief, not a wall of text:

```
RESUMED: bunchshop-pricing-deck — 2026-06-16
────────────────────────────────────────
WHERE WE LEFT OFF:
Mid-row 3 of the pricing table, Farmley 100g column. Need that
SKU price before filling the cell.

FIRST ACTION:
Find Farmley 100g SKU price, then complete rows 4–5.

WATCHOUTS:
None — handoff is 2 hours old.
────────────────────────────────────────
Ready. Say "go" to continue or give me a new direction.
```

From there, Claude works as if the conversation never ended — applying your vocabulary, constraints, and decisions silently, without re-explaining them.

---

## 📂 Where handoffs are stored

| Environment | Path |
|---|---|
| Cowork | `{project_root}/.handoffs/` (preferred) or `~/.claude/handoffs/` (fallback) |
| Claude Code | `.claude/handoffs/` in the project root, or `~/.claude/handoffs/` globally |
| Claude.ai | No filesystem — generated as an artifact, you save and paste it manually |

Filenames follow `YYYY-MM-DD-HHMMSS-{slug}.md`, where the slug is an auto-generated 2–3 word session summary (e.g. `ghosts-pwa-auth`).

Every handoffs folder maintains an **index** (`index.md`) tracking every handoff ever created, with status:

```
⏳ active  →  ✓ resumed  →  📦 archived
```

History is never deleted — it's the whole point.

---

## 📁 Repo structure

```
handoff-pro/
├── SKILL.md                       # Skill definition Claude reads to activate this skill
├── references/
│   └── handoff-template.md        # Exact document template used when generating a handoff
└── README.md                      # You are here
```

---

## 🛠 Customizing

- **Change storage location:** edit the `Storage paths` table in `SKILL.md`.
- **Add/remove captured fields:** edit the 12-element list in `SKILL.md` and the corresponding sections in `references/handoff-template.md`.
- **Tune proactive nudging:** adjust the exchange-count threshold under "Proactive Triggering" in `SKILL.md`.

---

## 📄 License

MIT — use it, fork it, adapt it for your own workflows.
