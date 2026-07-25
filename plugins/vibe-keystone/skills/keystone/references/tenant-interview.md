# The tenant interview

Ask before drafting. A keystone encodes somebody's conventions, and this skill's defaults are 626Labs-flavored. For any other tenant those defaults are wrong and must be replaced, not inherited by accident.

Ask in one round, grouped. Let them skip what does not apply.

## 1. Whose repo is this?

626Labs, another organization, or an individual project.

**If 626Labs:** the defaults apply. Skip to question 4.

**If another tenant:**
- What is the organization or individual name? It appears throughout the produced file.
- **Do you have tenant docs I should read before drafting?** A global `~/.claude/CLAUDE.md` defining a persona, an org handbook, a `CONTRIBUTING.md`, a values or principles doc, a brand voice guide, a style guide, an architecture doc. Name the paths.

## 2. Where do significant decisions log?

- A decision-log MCP, if the org runs one. Name it.
- A `decisions.md` or `docs/decisions/` in the repo.
- An external tracker — Linear, Jira, Notion, GitHub Issues.
- None. Decisions live in commit messages and PRs.

## 3. Persona

- **Inherit** — a global file defines a persona this repo should reference.
- **Override** — this repo needs its own, superseding global.
- **None** — no named persona. Drop the block.

## 4. Primary work mode

Code platform, marketing or content, long-form writing, or infrastructure and mixed. This shapes what the gotchas look like. See [`repo-types.md`](repo-types.md).

## 5. Existing `.claude/agents/`?

List them, or ask for proposals based on the repo.

## If a CLAUDE.md already exists

Ask whether to keep the current persona and voice and refresh the rest, or rewrite section by section.

Then check **what owns the file** before drafting anything — see [`skeleton.md`](skeleton.md). A generated keystone is edited at its template, not its output.

## Inherited docs are read twice

This is the half that is easy to miss.

**As a source.** Tenant priorities, voice rules, and conventions fold into the produced file so it reflects their world instead of this skill's defaults.

**As an exclusion list.** Anything already stated in an inherited file does not get restated. It gets a pointer.

A repo keystone that repeats the global file does not reinforce it. It creates two copies that drift, and the agent reading both has to work out which one wins. That is a real cost paid on every task.

Subject to [`protected-content.md`](protected-content.md) prong 3: confirm a canonical copy exists elsewhere before treating anything as duplicated. If this repo *is* the canonical home, it stays.

## Substitutions

| Section | 626Labs default | Other tenant |
|---|---|---|
| Voice and brand | 626Labs voice rules and tokens | Pull from their docs. No docs, no invented rules — propose a minimal block and ask. |
| Design system pointer | `~/.claude/skills/626labs-design/` | Their equivalent, or drop the pointer. |
| Decisions log | Auto-detected decision-log MCP | Whatever they named in Q2. "None" drops the section. |
| Persona | The Architect | Their persona name, or no persona. |

Never emit text implying a specific MCP is required. Auto-detect when present, name a fallback, treat "none" as first-class.
