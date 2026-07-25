---
name: keystone
description: Write a repo's CLAUDE.md so it earns every line — gotchas, rationale, and non-standard conventions, not a directory listing the agent could produce itself. Interviews for tenant context first, so the file encodes THEIR conventions rather than baked-in defaults. Use when starting in a repo without a CLAUDE.md, or when the existing one has gone fat or stale. Trigger phrases include "set up CLAUDE.md", "create the keystone", "bootstrap claude md", "claude md for this repo", "/keystone".
---

# Keystone

You are writing the `CLAUDE.md` that every agent decision in this repo will rest on. Get it right.

The question is not "which sections does this repo need." It is **what would a competent agent get wrong after reading this repo?** Everything else is the agent's own `ls` read back to it, and every session pays for it.

## The gate

Two questions per line. **Both must be yes to cut:**

1. **Derivable?** Could a session get this from `ls`, reading a file, reading the manifest, or `--help`?
2. **Default correct?** If the line were gone, would the model's default assumption be right?

Uncertain on the second means keep. Full rule, provenance, and worked examples: [`references/derivability-test.md`](references/derivability-test.md).

## The guard

The gate governs facts from the codebase. It has **no authority** over persona, voice, taste, priorities, or brand — that content fails the gate by construction, which is exactly why it is worth writing down. Never cut it for length; route it by how often it is needed. Inside a persona, identity is not procedure. Three prongs: [`references/protected-content.md`](references/protected-content.md).

## Flow

### 1. Inventory — refuse to write blind

`git status`, `git log --oneline -20`, top-level layout, the stack manifest, the README, `.claude/`, `.github/workflows/`, `docs/`.

**Find out what owns any existing `CLAUDE.md` before planning to touch it** — generated-file headers, sibling templates, render scripts, tool-owned marker regions. A diff against a generated file looks completely normal and the work vanishes on the next render. See [`references/file-ownership.md`](references/file-ownership.md).

Detect multi-surface repos here: workspace manifests, multiple app roots, distinct deploy targets. Verify surfaces against the actual tree; worktrees and archives are not surfaces.

Classify the repo type to aim the search — [`references/repo-types.md`](references/repo-types.md).

### 2. Interview

Whose repo, where decisions log, persona inheritance, work mode, existing agents. Read any tenant docs they name **twice**: as a source, and as an exclusion list. [`references/tenant-interview.md`](references/tenant-interview.md).

### 3. Draft

Seven sections, gate every line, route what passes but is not needed every task: [`references/skeleton.md`](references/skeleton.md) and [`references/progressive-disclosure.md`](references/progressive-disclosure.md).

Target ~50 lines, ceiling ~100, for the root file. The ceiling is soft and a gotcha is never cut to hit it — the budget squeezes derivable content only.

If nothing survives for gotchas, conventions, or rationale, render the **declined verdict** and write nothing. Some repos do not need a keystone yet.

### 4. Self-check

- [ ] Every line passes the gate, or is protected content
- [ ] Protected content was relocated, never deleted, and its pointers resolve
- [ ] Nothing restates the inherited global or tenant file, unless this repo is its canonical home
- [ ] Root file is within budget, or the overflow has a named destination
- [ ] Gotchas is non-empty and every item names a real failure mode
- [ ] Multi-surface repos got a nested-keystone proposal
- [ ] No snapshot lists that rot — describe how to find state, never enumerate it
- [ ] Every referenced path exists on disk
- [ ] If the existing file carried sections this skeleton no longer produces, the builder was told **before** the diff, not after

### 5. Propose, don't create

Write the root `CLAUDE.md` and nothing else. Nested keystones, skills, `.claude/agents/`, `.claude/rules/`, and `.claude/hooks/` are proposals the builder accepts or declines.

### 6. Capture

Opt-in, default off, local only. Ask once, after the file lands: [`references/capture.md`](references/capture.md).

## Output

`CLAUDE.md` at repo root. ATX headings. Terse, action-first, second person. Code fences for commands, tables for anything tabular. No emoji. Em-dashes minimal — commas, periods, colons carry most of the load.

## What you do not do

- **Don't write blind.** Inventory first. Blind writes produce generic files.
- **Don't write over a generated file.** Retarget its template, leave tool-owned regions alone, and say so before writing.
- **Don't restate the global persona** unless explicitly overriding. The global file already loads; this one adds project context.
- **Don't enumerate current state.** No "recent decisions", no "current sprint", no counts that drift. Describe how to find state.
- **Don't cut protected content to hit a budget.** Relocate it and leave a pointer.
- **Don't manufacture guardrails to fill a section.** There is no minimum. "Don't commit secrets" is not a gotcha.
- **Don't auto-create anything but the root file.**
- **Don't overwrite an existing `CLAUDE.md`** without showing a diff and confirming.
- **Don't rightsize an existing file in place** — that is `/doctor`'s job. Keystone regenerates from the repo, which means it can surface a trap nobody wrote down; `/doctor` trims the file, which means it can only remove what is already there. Different inputs, different jobs.
