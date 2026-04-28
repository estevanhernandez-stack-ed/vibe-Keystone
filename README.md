# vibe-Keystone

> *If your `CLAUDE.md` surface is weak, your entire architecture could crumble down.*

The keystone is the load-bearing structural file in any repository that uses Claude Code: every agent decision, every dispatched subagent, every commit message rests on it. **vibe-Keystone** is the bootstrap skill that produces a 626Labs-pattern `CLAUDE.md` after inventorying a repo, confirming scope, and adapting per repo type.

You hand a Claude Code session this skill, and it asks you four questions, then writes a `CLAUDE.md` that future agents in that repo will stand on.

---

## What it does

When invoked (typed as `/keystone` or triggered by phrases like *"set up CLAUDE.md"*, *"bootstrap claude md"*, *"create the keystone"*, *"claude md for this repo"*), the skill:

1. **Inventories the repo** — `git status`, `git log`, top-level layout, stack files (`package.json` etc.), existing `.claude/`, `.husky/`, workflows. Refuses to write blind.
2. **Confirms scope** with four short questions: Is this 626Labs-owned? Does the global persona apply or override? Primary work mode? Existing project-local agents to reference?
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
   - References (.claude/agents, .claude/rules, .claude/hooks)
4. **Self-checks** — verifies every required section is present, no rotting snapshot lists, voice rules match repo type, references are concrete (not generic).
5. **Proposes follow-ups** — `.claude/agents/`, `.claude/rules/`, `.claude/hooks/` candidates that fit the repo. Does not auto-create them.

---

## Repo-type adaptations

The skill produces a different shape per repo type:

| Repo type | Adaptations |
|---|---|
| **Code platform** (services, apps, libraries) | Tech Stack only; domain section covers MCP / infrastructure / federation patterns; conventions emphasize type safety, testing, styling |
| **Marketing / content site** | Tech Stack & Voice combined; voice rules are load-bearing; adds Design system section pointing at `~/.claude/skills/626labs-design/`; domain section covers rebuild pipeline, bot workflows, asset flow |
| **Long-form writing / thesis** | Persona override clause is mandatory; adds Citation discipline section; mode switching if template-driven; academic-flavored commits (`draft / revise / cite / respond / meta`) |
| **Infrastructure / mixed** | Tech Stack covers all surfaces; domain section explains the runtime model and cross-surface coordination |

---

## Installation

Two paths, depending on how you want to use it.

### Path A — User-level skill (recommended for daily use)

Drop the skill into your global Claude Code config so `/keystone` dispatches cleanly with no namespace prefix:

```bash
mkdir -p ~/.claude/skills/keystone
curl -sL https://raw.githubusercontent.com/estevanhernandez-stack-ed/vibe-Keystone/main/skills/keystone/SKILL.md \
  -o ~/.claude/skills/keystone/SKILL.md
```

Or clone + copy:

```bash
git clone https://github.com/estevanhernandez-stack-ed/vibe-Keystone.git
cp vibe-Keystone/skills/keystone/SKILL.md ~/.claude/skills/keystone/SKILL.md
```

Restart Claude Code (or open `/hooks` to refresh config) and you should see `keystone` in the skill listing. Then type `/keystone` in any new repo to invoke.

### Path B — Claude Code plugin marketplace

Add this repo as a marketplace in your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "vibe-keystone": {
      "source": {
        "source": "github",
        "repo": "estevanhernandez-stack-ed/vibe-Keystone"
      }
    }
  }
}
```

Then enable the plugin via `/plugin install vibe-keystone@vibe-keystone` (or however you've configured plugin installs). The skill dispatches as `/vibe-keystone:keystone` when installed this way — slightly more verbose than Path A but auto-updating with the repo.

---

## What it does NOT do

- **Does not write `CLAUDE.md` blind.** Inventories first.
- **Does not re-establish the global persona.** Project context layers *on top of* the global `CLAUDE.md` (e.g., 626Labs's "The Architect" or whatever you have).
- **Does not include rotting snapshot lists.** No "recent decisions" / "current sprint" enumerations — only how to *find* current state.
- **Does not auto-create `.claude/agents/`, `.claude/rules/`, `.claude/hooks/`.** Proposes them as follow-ups; you decide.
- **Does not overwrite an existing `CLAUDE.md`** without showing a diff and confirming.

---

## Why this exists

After bootstrapping CLAUDE.md files for four repos by hand (626Labs Dashboard, 626 Labs Hub, ThesisStudio, the global `~/.claude/`), a consistent pattern emerged. The pattern was clear; the act of remembering it across sessions and repos was tedious. This skill codifies the pattern so future-you (or any agent on your behalf) can apply it consistently.

The 626Labs project-CLAUDE convention is opinionated — voice rules for content-bearing repos, decisions-log discipline pointing at the 626Labs Dashboard, persona-override clauses for writing repos, federation-pattern compliance for code platforms. The skill bakes those opinions in so they don't get lost in translation.

---

## Brand context

This is a 626Labs internal-pattern skill, but the structure generalizes. If you're applying it outside 626Labs, the parts that need swapping:

- **Decisions log** points at 626Labs Dashboard MCP (`mcp__626Labs__manage_decisions log`). Replace with your own decision-tracking surface (or remove entirely).
- **Voice rules** assume the 626Labs brand voice (builder-to-builder, no corporate speak, em-dashes welcome, *"Imagine Something Else"* tagline). Replace with your own voice rules for content-bearing repos.
- **Brand tokens** for visual repos (cyan `#17d4fa` + magenta `#f22f89`, navy `#0f1f31`, Space Grotesk / Inter / JetBrains Mono). Swap in your own.

The skeleton (sections, ordering, "earn the folder" discipline, "What NOT to do" pattern) is brand-agnostic and works across teams.

---

## License

MIT (or whatever you'd like to apply — repo doesn't have a LICENSE file yet; add one before sharing widely).

---

*Built by [Estevan Hernandez](https://github.com/estevanhernandez-stack-ed) at 626 Labs. Part of the [vibe-* plugin family](https://626labs.dev) — `vibe-doc`, `vibe-cartographer`, `vibe-test`, `vibe-thesis`, `vibe-Keystone`.*
