# Vibe Keystone v0.3 — the derivability rewrite

## Idea

Re-cut the CLAUDE.md that Keystone generates against a two-axis gate — **cut what Claude can derive, keep what Claude would otherwise guess wrong** — and make Keystone's own SKILL practice the progressive disclosure it teaches.

## Who It's For

**Primary:** anyone running `/keystone` in a repo driven by a Claude 5 generation model. Today that is every user of the plugin, since the model generation changed underneath the tool.

**The specific unmet need:** Keystone's skeleton was calibrated when models needed to be told what they could already see. That calibration inverted. Its three ALWAYS sections — "What's where", "Tech Stack", and the domain/architecture section — are precisely the three that Claude Code's own `/doctor` now names as cut targets. The three `/doctor` says to keep (gotchas, rationale, non-standard conventions) have no dedicated section in the skeleton at all. Every keystone the tool ships today is born fat, and every session in that repo pays the token cost forever.

**Secondary:** Este's own estate, where six descendants of this pattern run 90 to 302 lines and average roughly 3:1 inventory-to-gotcha.

## Inspiration & References

- **The shipped `/doctor` implementation** — read out of `claude.exe` v2.1.220, not from a description of it. The authoritative criterion: *"trim checked-in CLAUDE.md files by cutting content a session could derive from the codebase (directory layouts, tech-stack lists, architecture overviews) while keeping gotchas, rationale, and non-standard conventions; migrate always-loaded CLAUDE.md guidance into lazy skills and nested CLAUDE.md files."* Its derivability test: *"(`ls`, `cat`, reading the manifest, `--help`) is dead weight every session it loads into pays for."*
- **The new rules of context engineering for Claude 5 generation models** — https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models — *"Keep your CLAUDE.md lightweight and briefly describe what your repo is for, but spend most of the tokens on gotchas inside of the codebase."* And on skills: *"For long skills, try and use progressive disclosure as much as possible — divide it into many files and split them out."*
- **A field guide to Claude Fable: finding your unknowns** — https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns — surfaced during `/scope` research and it changed the design. Two passages are load-bearing:
  - *"If you are too specific, Claude will follow your instructions even when a pivot may be more appropriate. If you are too vague, Claude will often make choices and assumptions based on industry best practices that may not be a fit for your task."*
  - The four-quadrant frame, of which **Unknown Knowns** — *"obvious-until-you-see-it details"* — is the exact definition of a gotcha.
- **Family operating doctrine** — `vibe-plugins/docs/conventions/operating-doctrine.md`. *"A model's tier sets its instincts. A written procedure is tier-portable."* In tension with frontier-first trimming; resolved by progressive disclosure rather than by picking a side.
- **`vibe-Keystone/PRIVACY.md`** — zero scripts, zero network, fully determined by SKILL files. A selling point and a hard constraint.

**Design energy:** none. Markdown-only plugin, no visual surface. The applicable taste is prose discipline: punchline first, specific over generic, no corporate speak, em-dashes minimal, no emoji in file content.

## Goals

1. **A keystone born lean.** Files that pass the gate at write time instead of needing `/doctor` to rescue them later.
2. **Stop trimming the irreplaceable.** Persona, voice, taste, and priorities are the one class of content a model cannot reconstruct from a repo. The guard makes cutting them a named error rather than an accident of enthusiasm.
3. **Teach by example.** Keystone's own SKILL drops from 390 lines to roughly 100 plus a reference tree. A tool that teaches progressive disclosure while ignoring it is an argument against itself.
4. **Prove it on real files.** Five estate keystones migrated. Hand-corrections are skeleton defects, not migration details.
5. **Stay honest about `/doctor`.** Keystone births, `/doctor` maintains. No overlap built, and the positioning rests on the shipped binary rather than a guess.

**What would make this worth doing:** the next repo that gets a keystone gets one that earns every line, and the person reading it in six months finds the trap that would have cost them an afternoon instead of a directory listing they could have run themselves.

## What "Done" Looks Like

- `/keystone` in a fresh repo produces a root file inside the ~50-line target, whose content is gotchas, rationale, and non-standard conventions, with pointers to where the depth lives.
- Multi-surface repos get a nested-keystone proposal instead of one fat root.
- Persona and voice content survives the rewrite in every case, relocated by frequency of need when it is situational.
- Keystone's SKILL is ~100 lines; the depth lives in seven reference files; every pointer resolves and no reference file is orphaned.
- Five estate CLAUDE.md files migrated, each with the diff reviewed before it was written.
- A friction log naming every hand-correction the skeleton required.
- v0.3.0 tagged on the solo repo, then the marketplace ref bumped. In that order.

## What's Explicitly Cut

- **No executable validator.** Proposal #1 from the parked harness doc stays parked. `PRIVACY.md` promises zero scripts and that promise is worth more than a linter. All checks stay agent-run.
- **No rightsizing capability.** `/doctor` already does it, verified. Building a second one would be duplicated surface with a worse maintenance story.
- **No lean/full mode toggle.** Considered and rejected during the brainstorm. Two skeletons to maintain, a selection burden on every user, to preserve a shape we believe is wrong.
- **No auto-writing beyond the root `CLAUDE.md`.** Nested keystones, skills, agents, rules, and hooks are proposed. Carried forward from v0.2.1 deliberately: this cycle's own migrations must honor the rule they are validating.
- **`.claude-personal/CLAUDE.md` is out of the migration set.** It is a persona file, not a repo keystone. Under the guard it is almost entirely protected content, so migrating it by the same yardstick would shred the thing the guard exists to protect. Separate judgment pass, Este-owned.
- **No port of Cartographer's full evolution stack.** Keystone runs once per repo. The Tier 0 capture plus Tier 1 `evolve-keystone` pair already shipped is the right ceiling.

The engineering-grounded cut, distinct from the MVP cuts above: **the validator is cut because the invariant is more valuable than the feature.** A tool that promises "no scripts, no network, fully determined by SKILL files" and then ships a Node linter has broken something users chose it for. That is an architectural invariant, not a scope trim.

## Loose Implementation Notes

**The gate is two-axis, not one.** The `/scope` research changed this. Axis one is derivability: could a session get this from `ls`, `cat`, the manifest, or `--help`? Axis two is default-correctness: if this line were absent, would the model's industry-default assumption be right? Cut requires *derivable* AND *default-would-be-right*. Anything where the default would be wrong stays, even when technically derivable.

The v1 single-axis version would have cut "the load-bearing artifact is `.claude-plugin/marketplace.json`" — technically derivable by reading everything, but the default assumption (README or `package.json` is the center) is wrong. Single-axis was going to trim load-bearing content.

**Seven sections:** Orientation, Gotchas, Non-standard conventions, Rationale, Pointers, Decisions log, What NOT to do. Gotchas is the new center of gravity, and the Fable guide gives it a definition worth putting in the skill: a gotcha is an **unknown known** — obvious once named, invisible until then.

**Budget** ~50 lines target, ~100 ceiling for a root file. Overflow relocates to a nested keystone or a skill; it never gets deleted. The budget is a forcing function for the gate, not an independent rule.

**Keystone's SKILL** → ~100 lines plus `references/`: derivability-test, protected-content, skeleton, progressive-disclosure, tenant-interview, repo-types, capture.

**Build ordering is a known trap.** The inbound plan builds everything then migrates everything. The builder profile records this failing three times — lessons (bbb) dogfood earlier, (ccc) wire-as-you-build, and "structural-green != works." The first real migration moves up to directly after the skeleton reference lands. Resolved at `/checklist`.

**Ship:** v0.3.0 on the solo repo, tag, verify the tag and the manifest path resolve at that tag, then bump the marketplace ref. Three descriptions carry the old framing and all three change together: `plugin.json`, the marketplace entry, and the SKILL frontmatter.
