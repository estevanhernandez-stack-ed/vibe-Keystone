---
name: keystone
description: Bootstrap a 626Labs-pattern CLAUDE.md for a repository. The keystone is the load-bearing structural file — every agent decision in the repo rests on it. Use when starting in a new repo without a CLAUDE.md, or when the existing one is stale. Trigger phrases include "set up CLAUDE.md", "create the keystone", "bootstrap claude md", "claude md for this repo", "/keystone".
---

# Keystone — bootstrap a 626Labs-pattern CLAUDE.md

You are an agent in a 626Labs-owned (or 626Labs-style) repository. Your task is to produce a `CLAUDE.md` for this repo following the 626Labs project-CLAUDE pattern. Read this entire skill before writing anything.

The keystone is the load-bearing structural file: every decision the agent makes in the repo rests on it. Get it right.

## Step 0 — Refuse to write blind

Before producing the file, inventory the repo:

1. `git status` and `git log --oneline -20` — branch state + recent rhythm
2. `ls -la` — top-level layout
3. Read `package.json`, `pyproject.toml`, `Cargo.toml`, or whatever defines the stack
4. Read existing `README.md` and any top-level `*.md` files
5. Check for: `.claude/`, `.husky/`, `.github/workflows/`, `scripts/`, `docs/`, `.ai/`
6. Look for build/deploy infra: `Makefile`, `Dockerfile`, GitHub Actions, etc.
7. **If `CLAUDE.md` already exists**, read it before overwriting. Do not blindly replace.

Identify the repo type:

- **Code platform** (services, apps, libraries, plugins)
- **Marketing/content site** (public-facing, copy-heavy)
- **Long-form writing / thesis** (prose-heavy, citation-bound)
- **Infrastructure / mixed** (multiple surfaces)

## Step 1 — Confirm scope before drafting

Ask the user (one round, concise):

1. Is this 626Labs-owned, or 626Labs-adjacent?
2. Does the global persona (`~/.claude/CLAUDE.md` — typically The Architect) apply, or does this repo need a persona override (e.g., Lead Writer for thesis work)?
3. What's the primary work mode: code, marketing/content, writing, infrastructure, mixed?
4. Existing `.claude/agents/` to reference — yes / no / propose new?

If the existing CLAUDE.md is being refreshed (not bootstrapped fresh), also ask: keep the existing persona/voice/structure, or rewrite section-by-section?

## Step 2 — Skeleton (sections in this order)

Produce a `CLAUDE.md` with these sections. Drop sections marked CONDITIONAL when they don't apply.

### 1. Title + persona inheritance note (ALWAYS)

```markdown
# {Repo Name}

> **Persona:** This repo inherits {global persona name} from `~/.claude/CLAUDE.md`. No need to re-establish — just adds project context below.
```

If the repo overrides the global persona (writing/thesis case), say so explicitly:

```markdown
> **Persona override:** In this repo, you operate as {Project Persona} — not the global one. {Persona} supersedes for {scope}; global process habits (gather context, log decisions, assess blast radius) still apply to project work (commits, file moves, MCP calls).
```

### 2. Tech Stack (ALWAYS) — paired with Voice for content-heavy repos

For a code-only repo:

```markdown
## Tech Stack

- **Language/Framework:** {actual}
- **Build:** {actual}
- **Deploy:** {actual}
- **Testing:** {actual}
```

For any public-facing / content-bearing repo, add a Voice section:

```markdown
## Tech Stack & Voice

- **Stack:** {as above}
- **Brand:** Cyan `#17d4fa` + magenta `#f22f89`, always paired. Navy `#0f1f31` field. Space Grotesk display, Inter body, JetBrains Mono code/meta (uppercase + 0.12em tracking on small labels).
- **Voice:** Builder-to-builder, second person, sentence case. No "empower / leverage / seamlessly / unlock / unleash." Em-dashes welcome. No emoji in UI copy or marketing surfaces. Tagline: *Imagine Something Else.*
```

### 3. Design system reference (CONDITIONAL — visual repos)

```markdown
## Design system

Canonical brand spec lives at `~/.claude/skills/626labs-design/` (globally available — same skill across every 626 Labs repo). Use `colors_and_type.css` as the token source and `ui_kits/` as the pattern reference. Local `Design/` (or wherever this repo keeps brand artifacts) is for repo-specific references only.
```

### 4. What's Where (ALWAYS)

A markdown table with paths and one-line purposes. One row per major directory or load-bearing file. Anyone should find anything in 5 seconds.

```markdown
## What's where

| Path | What it is |
|---|---|
| `src/` | {one line} |
| `scripts/` | {one line} |
| ... |
```

### 5. Domain-specific section (CONDITIONAL — every repo has one)

This is the operational center of gravity. Pick what applies:

- **Code platform:** MCP server / infrastructure / API surface / federation pattern
- **Site:** How the site rebuilds, bot workflows
- **Writing:** Mode switching (THESIS_MODE), citation discipline
- **Mixed:** A "How the system works at runtime" overview

Use sub-headings under one parent section. Be concrete — name the actual files, scripts, workflows, and triggers.

### 6. Common tasks (ALWAYS)

```markdown
## Common tasks

| You want to… | Path / command |
|---|---|
| {task} | {how} |
```

5–10 rows covering the most frequent operations.

### 7. Conventions (ALWAYS)

```markdown
## Conventions

- **Commits:** Conventional commits ({list types — for thesis repos use academic-flavored: draft / revise / cite / respond / meta / chore})
- **Style:** {brief, stack-specific}
- **File rules:** {what's read-only, what's generated, what's the canonical source}
```

### 8. Decisions log (ALWAYS)

```markdown
## Decisions log

Significant decisions log to the **626Labs Dashboard** via MCP (`mcp__626Labs__manage_decisions log`). Tag with the bound project ID. The bar: *would future-you (or someone asking "why this approach?") want to know this in 3–6 months?*

Especially:
- {category 1, repo-specific}
- {category 2}
- {3–5 categories total}

Skip the routine: {what doesn't get logged}.

If unbound (no 626Labs project): tag with the repo name in the description and set `projectId: null`.
```

### 9. What NOT to do (ALWAYS)

3–5+ explicit, repo-specific guardrails. Each one names the failure mode and (where useful) the right alternative.

```markdown
## What NOT to do

- **Don't {specific thing}** — {why, or what to do instead}
- ...
```

Examples to draw from when identifying don'ts: don't hand-edit generated files (and the right edit point), don't bypass the build pipeline, don't commit secrets, don't force-push to main, don't fabricate citations (thesis), don't write to read-only directories.

### 10. References (CONDITIONAL — when any of these exist)

```markdown
## References

- Architecture details: {path}
- Modular rules: `.claude/rules/`
- Specialized agents: `.claude/agents/` (see `agents/README.md`)
- Session hooks: `.claude/hooks/`
- CI/CD: `{path}`
```

## Step 3 — Output discipline

- File: `CLAUDE.md` at repo root
- Headings: ATX (`#`, `##`, `###`)
- Prose: terse, action-first
- No emoji in file content
- Em-dashes welcome
- Code fences for commands; markdown tables for paths/tasks/conventions
- Voice: builder-to-builder, second person, no hedging, no corporate speak

## Step 4 — Self-check before claiming done

Verify:

- [ ] Every ALWAYS section is present
- [ ] At least 3 explicit "What NOT to do" items, each repo-specific
- [ ] References specific paths/files in this repo (not generic placeholders)
- [ ] Decisions log section points at 626Labs MCP
- [ ] No re-establishing the global persona (unless explicitly overriding)
- [ ] No snapshot lists ("recent decisions", "current sprint") that will rot
- [ ] Voice section present if repo has public-facing surface

## Step 5 — Propose follow-ups (don't auto-create)

After the CLAUDE.md lands, propose:

- `.claude/agents/` candidates that would serve recurring tasks in this repo (code-reviewer, security-auditor, copy-reviewer, visual-asset-reviewer, citation-checker, devils-advocate, story-editor, etc. — pick what fits)
- `.claude/rules/` files for specialized guidance that doesn't fit in CLAUDE.md
- `.claude/hooks/` for automation that closes a feedback loop (template-edit drift checks, type-check-after-edit, etc.)

Each suggestion is a proposal. The user decides what gets built.

---

## Repo type quick reference

**Code platform:**

- Tech Stack only (no Voice section unless platform also has marketing surface)
- Domain section: architecture, MCP, federation pattern, infrastructure
- Conventions emphasize type safety, testing, styling
- "What NOT to do": code-quality + deployment guardrails

**Marketing / content site:**

- Tech Stack & Voice combined (voice is load-bearing)
- Add Design system section
- Domain section: rebuild pipeline, bot workflows, asset flow
- "What NOT to do": content/build pipeline + brand discipline

**Long-form writing / thesis:**

- Persona override clause is mandatory
- Add Citation discipline section
- Mode switching (if template-driven)
- Academic-flavored commits (draft / revise / cite / respond / meta)
- "What NOT to do": fabrication + scope creep + manifest discipline

**Infrastructure / mixed:**

- Tech Stack covers all surfaces
- Domain section: runtime model + cross-surface coordination
- "What NOT to do": cross-surface coordination guardrails

---

## What you do not do

- Do not write CLAUDE.md without inventorying the repo first. Blind writes produce generic, useless files.
- Do not re-establish the global persona unless explicitly overriding. The global Architect / Stitch / whatever already loads; the repo CLAUDE.md adds project context, doesn't restate identity.
- Do not list "current state" or "recent decisions" snapshot-style. Those rot. Always describe how to *find* state, never enumerate the current values.
- Do not include voice rules without a public-facing surface to apply them to. Code-only platforms don't need the brand voice section.
- Do not auto-create `.claude/agents/`, `.claude/rules/`, or `.claude/hooks/`. Propose them; let the user decide.
- Do not overwrite an existing CLAUDE.md without showing a diff and confirming.
