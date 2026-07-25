# Vibe Keystone v0.3 — Product Requirements

## Problem Statement

Keystone generates the CLAUDE.md that every agent decision in a repo rests on, and its skeleton was calibrated for a model generation that needed to be told what it could already see. That calibration inverted. The three sections Keystone marks ALWAYS — "What's where", "Tech Stack", and the domain/architecture section — are the three Claude Code's own `/doctor` now names as cut targets, and the three `/doctor` says to keep have no section in the skeleton at all.

The cost is paid per session, forever, in every repo the tool has touched. Six descendants in this estate run 90 to 302 lines at roughly 3:1 inventory-to-gotcha. Every one of those sessions spends context re-reading a directory listing it could have produced with `ls`, and does not get told the thing that would have saved it an afternoon.

## User Stories

### Epic 1: The gate

- As an agent writing a keystone, I want a single decision rule for every candidate line so that I stop guessing which sections a repo "needs".
  - [ ] The gate is stated as two questions, both of which must be YES to cut: (a) could a session obtain this by running `ls`, reading a file, reading the manifest, or running `--help`? (b) if this line were absent, would the model's default assumption be correct?
  - [ ] Axis (a) is labeled mechanical; axis (b) is labeled judgment. The split is stated, not blurred.
  - [ ] The tie-break is explicit: when axis (b) is uncertain, keep the line.
  - [ ] The reference quotes `/doctor`'s criterion once, with the binary version it was read from, so a reader knows the rule is not a house opinion.

- As a maintainer reading the reference, I want worked examples so that the rule is calibrated rather than abstract.
  - [ ] At least seven worked rows, each with line, verdict, and why.
  - [ ] At least one row demonstrates derivable-but-keep (the axis-2 case), because a table of pure cuts would teach the single-axis rule by accident.

### Epic 2: The protected-content guard

- As a builder whose repo encodes voice and taste, I want the trim to be structurally unable to touch that content so that enthusiasm for brevity cannot destroy the one thing an agent cannot reconstruct.
  - [ ] Prong 1 states the gate has no authority over human-supplied context, and enumerates the categories: persona, identity, voice rules, tone, banned phrases, taste, priorities, values, brand tokens, cultural reference material.
  - [ ] Prong 1 states the reasoning, not just the rule: this content fails `ls`/`cat`/`--help` by construction, which is exactly why it is worth writing down.
  - [ ] Prong 2 routes protected content by frequency of need. Always-needed stays inline; situational relocates; nothing is deleted to hit a budget.
  - [ ] Prong 3 requires checking whether this repo is the canonical home of the content before treating it as duplicated.
  - [ ] A worked example shows prong 2 in action.

- As a user running Keystone on the repo that holds my canonical voice guide, I want dedup to point rather than delete so that the tool cannot destroy the source it is deduplicating against.
  - [ ] Running Keystone in a repo whose CLAUDE.md is the only copy of the voice rules leaves those rules in place.
  - [ ] Dedup emits an inheritance pointer only when a canonical copy exists elsewhere.

### Epic 3: The generated skeleton

- As an agent entering a repo cold, I want the keystone's tokens spent on what I would otherwise get wrong so that reading it changes my behavior.
  - [ ] Seven sections: Orientation (ALWAYS), Gotchas (ALWAYS), Non-standard conventions (CONDITIONAL), Rationale (CONDITIONAL), Pointers (ALWAYS), Decisions log (CONDITIONAL), What NOT to do (CONDITIONAL).
  - [ ] Gotchas carries a definition, not just a label: a gotcha is an **unknown known**, an obvious-once-named detail that is invisible until named.
  - [ ] The 3-item floor on "What NOT to do" is removed. Zero is valid and drops the heading.
  - [ ] "What's where", "Tech Stack", "Common tasks", "Design system", and "Voice" are no longer sections; the removals are stated explicitly so a reader of the new skeleton knows they were cut deliberately.
  - [ ] Exemplars are short and real. No `{placeholder}` fill-in-the-blank template blocks, which is the pattern v0.2.1 used a dozen times and which constrains output shape.
  - [ ] The persona-inheritance blockquote survives all three cases (inherit / override / none).

- As a builder in a repo with nothing non-obvious in it, I want the tool to say so rather than manufacture content.
  - [ ] When a repo yields no genuine gotchas, no non-standard conventions, and no rationale, Keystone renders a **"not yet worth a keystone"** verdict instead of emitting a skeleton of empty headings.
  - [ ] The verdict names what would make the repo worth one later.
  - [ ] The verdict is a first-class outcome, not an error.

### Epic 4: Progressive disclosure

- As a builder in a multi-surface repo, I want per-surface guidance that costs nothing when I am working elsewhere.
  - [ ] The loading model is stated accurately: root and `.claude/CLAUDE.md` always load; nested-directory files load only when working under that directory; `.claude/rules/*.md` also load.
  - [ ] Multi-surface detection runs during inventory, using named signals (workspace manifests, multiple app roots, distinct deploy targets).
  - [ ] Nested keystones are proposed, never auto-written.
  - [ ] The root-stays rule is stated: anything an agent must see on every task stays at the root regardless of which surface it describes.

- As a builder relocating situational content, I want the destination chosen by how the content gets reached, not by taste.
  - [ ] The reference states that nested keystones are **location-gated** and skills are **invocable from anywhere**, and that these are therefore not interchangeable destinations.
  - [ ] Protected-but-situational content defaults to a **skill**, not a nested keystone, unless the work it governs genuinely only happens inside that subtree.
  - [ ] Rationale is stated: marketing voice rules parked in a nested file under a marketing subtree fail silently the moment someone writes marketing copy from the repo root.

- As a builder with a genuinely complex repo, I want the budget to squeeze derivable content and never real content.
  - [ ] Budget is ~50 lines target, ~100 ceiling, for the root file.
  - [ ] The ceiling is explicitly soft: a repo with twenty real root-level gotchas correctly produces a file over 100 lines.
  - [ ] A gotcha is never cut to hit budget. Stated as a rule, not implied.

### Epic 5: Keystone's own SKILL

- As a reader of the skill, I want it to practice what it teaches so that the tool is not an argument against itself.
  - [ ] `SKILL.md` lands at roughly 100 lines, hard ceiling 130, down from 390.
  - [ ] Seven reference files: derivability-test, protected-content, skeleton, progressive-disclosure, tenant-interview, repo-types, capture.
  - [ ] Every `references/*.md` pointer in `SKILL.md` resolves to a file that exists.
  - [ ] Every reference file is linked from `SKILL.md`. No orphans.
  - [ ] The gate and the guard each appear in brief in `SKILL.md` — enough that an agent reading only `SKILL.md` still behaves correctly — with depth in the references.
  - [ ] The eight-item self-check stays inline, because it is needed on every run.
  - [ ] Frontmatter `description` is unquoted. Escaped inner quotes blank the in-session listing; that was fixed in `4e8bea6` and must not regress.

### Epic 6: Validation

- As the maintainer, I want the skeleton proven on real files before it ships so that "structural-green" does not get mistaken for "works".
  - [ ] Five estate keystones migrated: `vibe-plugins`, `Project-626Labs-1`, `Celestia3`, `vibe-cartographer`, `Projects`.
  - [ ] The first migration runs immediately after the skeleton reference lands, before the remaining references and the SKILL rewrite.
  - [ ] Every migration shows a diff and gets confirmation before the file is written.
  - [ ] Every hand-correction is recorded as a skeleton defect.
  - [ ] A recurring defect stops the migration run and fixes the reference tree before continuing.
  - [ ] Protected content survives every migration. Verified per file, not assumed.

### Epic 7: Ship

- As a marketplace user, I want the promotion to be safe so that a bad pin does not break installs.
  - [ ] `v0.3.0` in `plugin.json`, CHANGELOG entry, README rewritten where it describes the old shape.
  - [ ] All three descriptions change together: `plugin.json`, marketplace entry, SKILL frontmatter.
  - [ ] Tag exists and resolves on the remote before the marketplace `ref` moves.
  - [ ] The plugin manifest is verified present at that tag.
  - [ ] Promotion is linear: solo repo first, tag, then marketplace bump.

## What We're Building

Everything in Epics 1 through 7. The acceptance criteria above are the definition of done.

The load-bearing subset, if the cycle has to be cut short: Epics 1, 2, 3, and 5 are the product. Epic 4 is what makes it work on real repos. Epic 6 is what makes any of it trustworthy. Epic 7 is what makes it exist for anyone else.

## What We'd Add With More Time

- **A drift doctor.** Re-read a shipped keystone against the repo's current state and flag rot. Named and deferred in the parked harness proposals as Tier 2; it is arguably a different product and it would grow Keystone a command surface.
- **An executable validator.** The path-existence and placeholder-leak checks are mechanical and would be better as code. Blocked on the zero-scripts promise, deliberately.
- **A "name your own decision-log MCP" option** in the generated keystone, so a produced file can point at the repo owner's server tool names. Carried from the 2026-05-23 evolve signal, still deferred.
- **Capture-driven skeleton tuning.** Once `captures.jsonl` has 5+ v2 entries, `evolve-keystone` can start proposing section changes from evidence rather than judgment.

## Non-Goals

1. **Rightsizing existing files as a Keystone feature.** `/doctor` does this, verified in the shipped binary. Building a second implementation would be duplicated surface with a worse maintenance story.
2. **Shipping any executable.** `PRIVACY.md` promises no scripts and no network. The promise is worth more than the linter.
3. **A lean/full mode toggle.** Two skeletons to maintain and a selection burden on every user, in order to preserve a shape we believe is wrong.
4. **Auto-writing anything but the root `CLAUDE.md`.** Nested keystones, skills, agents, rules, and hooks stay proposals.
5. **Migrating `.claude-personal/CLAUDE.md`.** A persona file, almost entirely protected content. Migrating it by this yardstick would shred the thing the guard exists to protect.

## Open Questions

**Needs answering before `/spec`:**

1. **Regeneration versus trimming — where exactly is the `/doctor` line?** The five migrations in Epic 6 are, operationally, maintenance on existing files, which is the job assigned to `/doctor`. The distinction that resolves it: Keystone **regenerates** (re-derives a file from the repo, showing a diff) while `/doctor` **trims** (edits the existing file in place). Different operations on the same artifact. `/spec` must state this, because "Keystone births, `/doctor` maintains" is too clean a slogan to survive contact with the refresh case.

**Can wait:**

2. **What if the inherited global file is itself bloated?** Dedup points at the global file and inherits whatever is wrong with it. Proposed resolution: note it in the run output, do not fix it — the global file is `/doctor`'s territory and out of scope here.
3. **Path rot after write.** The self-check verifies referenced paths exist at write time; nothing verifies them later. That is the deferred drift doctor. Name the limitation in the skill rather than implying freshness is guaranteed.
4. **Where the "not yet worth a keystone" verdict routes.** Vibe-Walk shipped a first-class "don't build a tour" verdict and it was the differentiator. Same shape here. Open question is whether the verdict writes anything at all or purely reports.
