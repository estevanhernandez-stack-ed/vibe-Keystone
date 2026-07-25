# The skeleton

Seven sections. Two are always present; the rest appear only when the repo has something real to put in them.

Run every candidate line through [`derivability-test.md`](derivability-test.md) first, and check anything human-authored against [`protected-content.md`](protected-content.md) before cutting it.

## 1. Orientation — ALWAYS

Three to five lines. What the repo is for and its role among its siblings. Not derivable: a repo cannot tell you it is the marketplace front door rather than one of the plugins.

> Marketplace aggregation manifest for the Vibe plugin family. Plugin source lives in solo repos; this repo pins stable tags. Most commits here are a one-line ref bump.

## 2. Gotchas — ALWAYS

The center of gravity. Most of the file's tokens belong here.

**A gotcha is an unknown known** — obvious the moment it is named, invisible until then. Each item earns its line by naming a failure that happened, or would.

> - Editing the manifest and a solo repo in parallel breaks the promotion-is-deliberate invariant the channel model rests on. Ship on the solo first, tag, then bump here.
> - `data/stats/` is bot-owned. The workflow assumes it is the only writer.

If you cannot name the failure mode, it is not a gotcha. Cut it.

## 3. Non-standard conventions — CONDITIONAL

Only where the repo diverges from what a competent agent would assume. Zero is valid, and drops the heading.

> Tag naming: `vX.Y.Z` everywhere except `vibe-test` and `vibe-sec`, which use `<plugin>-vX.Y.Z` from their extraction history. Don't normalize without checking actual tags.

"Conventional commits" is not this. `git log` shows it in one command and it is the common default.

## 4. Rationale — CONDITIONAL

Why the repo is the way it is, specifically where an agent would otherwise "fix" something deliberate. Fold into Gotchas when thin.

> `packages/core/` ships stub implementations that throw. That is intentional — it is an interface skeleton downstream plugins pin against. Don't implement it to make tests pass.

## 5. Pointers — ALWAYS

The progressive-disclosure spine. Where the depth lives: skills, `docs/`, nested keystones, agents. This section replaces inlined architecture rather than summarizing it.

> - Promotion procedure: `docs/conventions/promotion-checklist.md`
> - Per-surface guidance: `web/CLAUDE.md`, `extension/CLAUDE.md`
> - Release ritual: the `cut-release` skill

## 6. Decisions log — CONDITIONAL

Where significant decisions land, and the bar for what counts. Follows the family decision-log-backend convention: auto-detected MCP when present, a file or tracker as fallback, "none" is first-class. Never emit text implying a specific MCP is required.

Drop the section entirely when the answer is "none."

## 7. What NOT to do — CONDITIONAL

Repo-specific, non-obvious guardrails only.

**There is no minimum.** Zero is a valid count and drops the heading. A floor manufactures filler, and "don't commit secrets" is filler — the model already knows.

> Don't pin a plugin's `ref` to `main` or a SHA. Stable means stable tags, or "stable" means nothing.

## Persona inheritance

One line at the top, above Orientation. Three cases, unchanged:

**Inherit:** `> **Persona:** This repo inherits {name} from ~/.claude/CLAUDE.md. No need to re-establish — just adds project context below.`

**Override:** `> **Persona override:** In this repo you operate as {Project Persona}, not the global one. {Persona} supersedes for {scope}; global process habits still apply.`

**None:** drop the blockquote. Just the title.

This is protected content and the model for correct dedup: point at the canonical definition, restate nothing.

## What is no longer a section

**What's where, Tech Stack, Common tasks, Design system, Voice.** Each survives only as residue that fails the gate, folded into Gotchas or Pointers.

A path row survives when it carries rationale — "the load-bearing artifact is `.claude-plugin/marketplace.json`" — not when it restates `ls`. A stack line survives when the pin is load-bearing and non-obvious, not because a stack exists.

Write real lines from the repo in front of you. Do not emit fill-in-the-blank templates with brace-delimited placeholder tokens; they constrain the shape of the output instead of communicating the criterion.

## When nothing survives: the declined verdict

Some repos do not need a keystone yet. A fresh scaffold with one file has no gotchas, no divergent conventions, and no rationale — everything true about it is derivable.

When classification yields nothing for sections 2, 3, and 4, **do not emit a skeleton of empty headings.** Report:

> This repo doesn't need a keystone yet. Everything an agent needs is derivable from the tree, the manifest, and `--help`. Worth revisiting once there's a trap worth naming: a build step with an order dependency, a directory something else owns, a convention that diverges from the obvious default.

**Write nothing.** The verdict is a first-class outcome, not an error, and not a stub file. A stub recreates the empty-skeleton problem the verdict exists to prevent.
