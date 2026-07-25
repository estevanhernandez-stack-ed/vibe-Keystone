# The derivability test

The gate every candidate line passes before it earns a place in a keystone.

## The gate

Two questions. **Both must be yes to cut.**

1. **Derivable?** Could a session obtain this by running `ls`, reading a file, reading the manifest, or running `--help`?
2. **Default correct?** If this line were absent, would the model's default assumption be right?

Anything else stays.

Axis 1 is **mechanical** — you can check it. Axis 2 is **inferential** — it is a judgment call. Keep the two labeled separately. Blurring them lets a judgment call present itself as a mechanical check, which is how over-trimming gets justified.

**Tie-break: uncertain on axis 2 means keep.** A gate that resolves ambiguity toward deletion will eventually delete something load-bearing.

## Where axis 1 comes from

Not a house opinion. Claude Code's own `/doctor` applies it (read from `claude.exe` v2.1.220):

> "trim checked-in CLAUDE.md files by cutting content a session could derive from the codebase (directory layouts, tech-stack lists, architecture overviews) while keeping gotchas, rationale, and non-standard conventions"

and on what counts as derivable:

> "(`ls`, `cat`, reading the manifest, `--help`) is dead weight every session it loads into pays for."

## Where axis 2 comes from

The other failure mode, from Anthropic's Fable field guide:

> "If you are too specific, Claude will follow your instructions even when a pivot may be more appropriate. If you are too vague, Claude will often make choices and assumptions based on industry best practices that may not be a fit for your task."

Thin context does not produce a model that asks. It produces one that quietly substitutes an industry default. Axis 2 is the check for whether that default would be wrong here.

## What survives

- **Gotchas** — traps, invariants, and surprises. A gotcha is an **unknown known**: obvious the moment it is named, invisible until then. Bias toward things that have actually bitten someone.
- **Rationale** — why the repo is the way it is, specifically where an agent would otherwise "fix" something deliberate.
- **Non-standard conventions** — only where the repo diverges from what a competent agent would assume by default.

## Worked examples

| Line | Axis 1: derivable? | Axis 2: default right? | Verdict |
|---|---|---|---|
| "Stack: Node ≥20, pnpm ≥9 workspace" | Yes — `cat package.json` | Yes | **Cut** |
| "`scripts/npm-stats.py` collects daily download counts" | Yes — the file says so | Yes | **Cut** |
| "Commits: conventional commits" | Yes — `git log` | Yes, it is the common default | **Cut** |
| "Thesis repos commit with `draft`/`revise`/`cite`/`respond`" | No | No — the default guess is conventional commits | **Keep** |
| "Don't reintroduce the `github` source type: it resolves SSH clone URLs and fails for users without keys" | No | No | **Keep** |
| "`data/stats/` is bot-owned. Don't hand-edit." | No | No — the default is that any file is editable | **Keep** |
| "All plugins tag `vX.Y.Z` except `vibe-test` and `vibe-sec`, which use `<plugin>-vX.Y.Z`" | Partly — `git tag` shows it | No — the divergence reads like a mistake to fix | **Keep** |
| **"The load-bearing artifact is `.claude-plugin/marketplace.json`"** | **Yes** — inferable by reading everything | **No** — the default assumption is that the README or `package.json` is the center | **Keep** |

That last row is why the gate has two axes. On derivability alone it gets cut, and the keystone loses the one line that tells an agent where the repo's center of gravity actually is.

## What this gate does not govern

Persona, voice, taste, priorities, brand tokens. That content fails axis 1 by construction, and cutting it for length is a category error. See [`protected-content.md`](protected-content.md) before trimming anything that came from a human rather than the codebase.
