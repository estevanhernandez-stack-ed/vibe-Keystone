<p align="center">
  <img src="assets/brand/icon.svg" width="120" alt="vibe-keystone mark">
  <img alt="Vibe Keystone — write a CLAUDE.md that earns every line" src="https://626labs.dev/assets/brand/plugins/vibe-keystone-banner-1500x500.png" />
</p>

# Vibe Keystone

**Writes a `CLAUDE.md` that earns every line. Cuts what your agent can already derive; keeps what it would otherwise get wrong.**

[![stable](https://img.shields.io/github/v/tag/estevanhernandez-stack-ed/vibe-Keystone?label=stable&color=17d4fa)](https://github.com/estevanhernandez-stack-ed/vibe-Keystone/tags)

The keystone is the load-bearing structural file in any repository that uses Claude Code — every agent decision, every dispatched subagent, every commit message rests on it. If your `CLAUDE.md` surface is weak, the architecture standing on it can crumble. You hand a Claude Code session this skill, it asks you a few questions, then writes a `CLAUDE.md` that future agents in that repo will stand on.

## What it does

When invoked — typed as `/keystone`, or triggered by phrases like *"set up CLAUDE.md"*, *"bootstrap claude md"*, *"create the keystone"* — the skill:

1. **Inventories the repo.** Git state, layout, stack manifest, existing `.claude/`, workflows. Refuses to write blind. Also works out **what owns any existing `CLAUDE.md`** — a generated file, or one carrying tool-owned regions, is edited at its source, not its output.
2. **Interviews you for tenant context.** Whose repo, where decisions log, persona inheritance, work mode. Any tenant docs you name get read twice: as a source, and as an exclusion list.
3. **Gates every line**, then writes what survives.
4. **Self-checks** against eight criteria, including that every referenced path exists on disk.
5. **Proposes follow-ups** — nested keystones, skills, agents, rules, hooks. Writes none of them.

## The gate

Two questions per candidate line. **Both must be yes to cut:**

1. **Derivable?** Could a session get this from `ls`, reading a file, reading the manifest, or `--help`?
2. **Default correct?** If the line were gone, would the model's default assumption be right?

The first axis is mechanical, and it is the criterion Claude Code's own `/doctor` applies. The second is judgment, and it exists because thin context does not produce a model that asks — it produces one that quietly substitutes an industry default. Uncertain on the second means keep.

What survives: **gotchas, rationale, and non-standard conventions.** A gotcha is an *unknown known* — obvious the moment it is named, invisible until then. If you cannot name the failure mode, it is not a gotcha.

What does not: directory layouts, tech-stack lists, architecture overviews, command tables. Your agent can run `ls`.

## What it will not cut

Persona, voice, taste, priorities, brand tokens. That content fails the gate by construction, which is exactly why it is worth writing down — it is the one class of thing an agent cannot reconstruct from your repo.

It is never deleted for length. It is routed by how often it is needed: always-needed stays inline, situational moves to a skill, and deduplication points at the canonical copy rather than deleting the last one. Inside a persona, identity and procedure are separated — a long persona is often a short identity carrying a lot of restated process.

## Progressive disclosure

Nested `CLAUDE.md` files load only while you are working under their directory, so multi-surface repos get a thin root plus per-surface proposals instead of one fat file. Guidance that is procedural and only sometimes needed becomes a proposed skill.

The root file targets ~50 lines with a ~100 ceiling. The ceiling is soft: a repo with twenty real gotchas correctly produces a longer file. The budget squeezes derivable content and nothing else.

## When it declines

Some repos do not need a keystone yet. When a repo yields no gotchas, no divergent conventions, and no rationale, the skill says so and **writes nothing** rather than emitting a skeleton of empty headings.

## What it does not do

- **Does not write blind.** Inventories first.
- **Does not overwrite a generated file.** Retargets its template and leaves tool-owned regions alone.
- **Does not restate the global persona**, or anything else the inherited file already says.
- **Does not manufacture guardrails.** There is no minimum. "Don't commit secrets" is not a gotcha.
- **Does not auto-create** nested keystones, skills, agents, rules, or hooks. It proposes; you decide.
- **Does not overwrite an existing `CLAUDE.md`** without showing a diff and confirming.
- **Does not rightsize files in place** — that is `/doctor`'s job. Keystone regenerates from the repo, so it can surface a trap nobody wrote down; `/doctor` trims the file, so it can only remove what is there. Different inputs, different jobs.

### Using outside your own org

The defaults are 626Labs-flavored and the interview swaps them out. Name your org, your decision-log surface, your persona, and any docs you already have — a global `CLAUDE.md`, a handbook, a voice guide — and they fold into the produced file instead of 626Labs conventions landing by accident. No tenant docs at all means a minimal block you are asked to confirm, never invented rules.

The part that generalizes is the gate, the guard, and the discipline. The content comes from your repo and your answers.

## Validated on

Five real keystones across the 626 Labs estate, which surfaced five defects that would otherwise have shipped — including that the skill would silently overwrite generated `CLAUDE.md` files, and that a repo can carry a 205-line `CLAUDE.md` containing nothing about itself.

| Repo | Before | After |
| --- | --- | --- |
| `vibe-plugins` | 147 | 46 |
| `Celestia3` | 205 | 140 (authored — it had no keystone at all) |
| `vibe-cartographer` | 204 | 152 |
| `Projects` | 90 | 52 |

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
