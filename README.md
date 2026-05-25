<p align="center">
  <img alt="Vibe Keystone — bootstrap a 626Labs-pattern CLAUDE.md for any repo" src="https://626labs.dev/assets/brand/plugins/vibe-keystone-banner-1500x500.png" />
</p>

# Vibe Keystone

**The bootstrap skill that writes a 626Labs-pattern `CLAUDE.md` for any repo — inventory first, interview for tenant context, adapt to the repo type.**

[![stable](https://img.shields.io/github/v/tag/estevanhernandez-stack-ed/vibe-Keystone?label=stable&color=17d4fa)](https://github.com/estevanhernandez-stack-ed/vibe-Keystone/tags)

The keystone is the load-bearing structural file in any repository that uses Claude Code — every agent decision, every dispatched subagent, every commit message rests on it. If your `CLAUDE.md` surface is weak, the architecture standing on it can crumble. You hand a Claude Code session this skill, it asks you a few questions, then writes a `CLAUDE.md` that future agents in that repo will stand on.

## What it does

When invoked — typed as `/keystone`, or triggered by phrases like *"set up CLAUDE.md"*, *"bootstrap claude md"*, *"create the keystone"*, *"claude md for this repo"* — the skill:

1. **Inventories the repo** — `git status`, `git log`, top-level layout, stack files (`package.json` etc.), existing `.claude/`, `.husky/`, workflows. Refuses to write blind.
2. **Interviews you for tenant context** — whose repo is this (626Labs / another org / individual)? Are there tenant docs (handbook, voice guide, principles, persona) the skill should read before drafting? Where do significant decisions log? Persona inheritance vs. override? Repo type? The defaults are 626Labs-flavored, but tenant answers swap them through. **No org's conventions get baked in by accident.**
3. **Produces a `CLAUDE.md`** following a structured skeleton:
   - Title + persona inheritance note (or override)
   - Tech Stack (paired with Voice for content-bearing repos)
   - Design system reference (for visual repos)
   - "What's where" path table
   - Domain-specific section (operational center of gravity per repo type)
   - Common tasks table
   - Conventions
   - Decisions log → 626Labs Dashboard MCP
   - "What NOT to do" guardrails
   - References (`.claude/agents`, `.claude/rules`, `.claude/hooks`)
4. **Self-checks** — verifies every required section is present, no rotting snapshot lists, voice rules match repo type, references are concrete (not generic).
5. **Proposes follow-ups** — `.claude/agents/`, `.claude/rules/`, `.claude/hooks/` candidates that fit the repo. Does not auto-create them.

## How it works

The skill adapts its output to the repo type — same skeleton, different shape:

| Repo type | Adaptations |
| --- | --- |
| **Code platform** (services, apps, libraries) | Tech Stack only; domain section covers MCP / infrastructure / federation patterns; conventions emphasize type safety, testing, styling |
| **Marketing / content site** | Tech Stack & Voice combined; voice rules are load-bearing; adds Design system section pointing at `~/.claude/skills/626labs-design/`; domain section covers rebuild pipeline, bot workflows, asset flow |
| **Long-form writing / thesis** | Persona override clause is mandatory; adds Citation discipline section; mode switching if template-driven; academic-flavored commits (`draft / revise / cite / respond / meta`) |
| **Infrastructure / mixed** | Tech Stack covers all surfaces; domain section explains the runtime model and cross-surface coordination |

**What it does not do:**

- **Does not write `CLAUDE.md` blind.** Inventories first.
- **Does not re-establish the global persona.** Project context layers *on top of* the global `CLAUDE.md` (e.g., 626Labs's "The Architect" or whatever you have).
- **Does not include rotting snapshot lists.** No "recent decisions" / "current sprint" enumerations — only how to *find* current state.
- **Does not auto-create `.claude/agents/`, `.claude/rules/`, `.claude/hooks/`.** Proposes them as follow-ups; you decide.
- **Does not overwrite an existing `CLAUDE.md`** without showing a diff and confirming.

### Using outside 626Labs

The skill auto-adapts. When you tell it your repo isn't 626Labs-owned, it asks for your tenant context up front (org name, decision-tracking surface, voice rules, persona, brand tokens) and threads those answers through the produced `CLAUDE.md` instead of inheriting 626Labs defaults.

What you can hand it:

- **Tenant docs you already have** — a global `~/.claude/CLAUDE.md`, an org `HANDBOOK.md`, a `CONTRIBUTING.md`, a values doc, a brand voice guide. Name the paths in your answer; the skill reads them before drafting and folds priorities in.
- **Or no tenant docs at all** — the skill writes a minimal-default block and asks you to confirm or extend rather than inventing rules.

Things you'll explicitly NOT get unless you ask for them:

- 626Labs Dashboard MCP references in the Decisions log section
- 626Labs voice rules ("no empower / leverage / seamlessly", em-dashes welcome, *"Imagine Something Else"* tagline)
- 626Labs brand tokens (cyan / magenta / navy + Space Grotesk / Inter / JetBrains Mono)
- The Architect or Stitch persona inheritance assumption

The skeleton itself (sections, ordering, "earn the folder" discipline, "What NOT to do" pattern, decisions-log discipline) is the part that generalizes — the *content* of each section comes from your tenant answers.

### Why this exists

After bootstrapping CLAUDE.md files for four repos by hand (626Labs Dashboard, 626 Labs Hub, ThesisStudio, the global `~/.claude/`), a consistent pattern emerged. The pattern was clear; the act of remembering it across sessions and repos was tedious. This skill codifies the pattern so future-you — or any agent on your behalf — can apply it consistently.

The 626Labs project-CLAUDE convention is opinionated: voice rules for content-bearing repos, decisions-log discipline pointing at the 626Labs Dashboard, persona-override clauses for writing repos, federation-pattern compliance for code platforms. The skill bakes those opinions in so they don't get lost in translation.

## Validated on

Bootstrapped the CLAUDE.md across every 626 Labs work repo and the main repo.

## Install

**Stable (recommended) — as a Claude Code plugin via the marketplace:**

```text
/plugin marketplace add estevanhernandez-stack-ed/vibe-plugins
/plugin install vibe-keystone@vibe-plugins
```

**Canary — track this repo's `main`:**

```text
/plugin install vibe-keystone@estevanhernandez-stack-ed/vibe-Keystone
```

Once installed, type `/keystone` in any new repo to invoke. As a marketplace plugin the skill dispatches as `/vibe-keystone:keystone` — slightly more verbose, but auto-updating with the repo.

## Part of the Vibe ecosystem

Part of the **[Vibe Plugins](https://github.com/estevanhernandez-stack-ed/vibe-plugins)** marketplace from [626 Labs](https://626labs.dev) — foundations and process pillars for AI-assisted creation. Keystone is a Foundation — structural: it sets the surface every other plugin's agents stand on.

```text
/plugin marketplace add estevanhernandez-stack-ed/vibe-plugins
```

## License

MIT — *Imagine Something Else.* See [`LICENSE`](LICENSE). Copyright (c) 2026 626Labs LLC (Estevan Hernandez).

---

*Built by [Estevan Hernandez](https://github.com/estevanhernandez-stack-ed) at 626 Labs.*
