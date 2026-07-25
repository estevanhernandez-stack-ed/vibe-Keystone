# Vibe Keystone v0.3 — Technical Spec

## Stack

Markdown. Nothing else.

Keystone is a Claude Code plugin whose entire runtime is SKILL files interpreted by the agent. No language runtime, no package manager, no build step, no tests to execute. `PRIVACY.md` states the invariant: *no scripts, zero outbound, fully determined by SKILL files.* That is a selling point and a hard constraint on this cycle.

| Dependency | Version | Why |
|---|---|---|
| Claude Code plugin format | `.claude-plugin/plugin.json` | The only loader-recognized manifest location. A root-level manifest silently degrades to auto-discovery with no metadata (vibe-prompt v0.7.0 incident). |
| Skill format | `skills/<name>/SKILL.md` + YAML frontmatter | `name` and `description`. `description` drives triggering. |
| `references/` subdirectory | — | **Verified this session, not assumed.** Shipped in `vibe-cartographer/plugins/vibe-cartographer/skills/guide/references/` (626 marketplace) and in Anthropic's own `superpowers/6.2.0/skills/using-superpowers/references/`. The progressive-disclosure restructure rests entirely on this pattern working; both precedents are live. |

Reference docs:
- Plugin structure: https://docs.claude.com/en/docs/claude-code/plugins
- Skills: https://docs.claude.com/en/docs/claude-code/skills

## Runtime & Deployment

**Runtime:** the agent's session. The skill is read and interpreted; nothing executes.

**Deployment target:** `vibe-plugins-marketplace` (from the unified profile). Per the per-target field lookup, this is the GitHub-release row plus a marketplace pin.

### Deployment — Identity & Signing

| Field | Value |
|---|---|
| Repo slug | `estevanhernandez-stack-ed/vibe-Keystone` |
| Release tag scheme | `vX.Y.Z` plain semver. **Not** the `<plugin>-vX.Y.Z` form — that variant belongs only to `vibe-test` and `vibe-sec`, rooted in their `git-filter-repo` extraction history. Existing tags confirm: `v0.1.0`, `v0.1.1`, `v0.2.0`, `v0.2.1`. |
| This release | `v0.3.0` |
| Signing | None. No cert, no sigstore. Consistent with the rest of the family. |
| Token scope | `gh` CLI ambient auth for tag verification. No secrets in the repo; nothing here needs `GITHUB_TOKEN` beyond what `gh` already holds. |
| Marketplace pin | `vibe-plugins/.claude-plugin/marketplace.json`, entry `vibe-keystone`, `source.source: git-subdir`, `source.path: plugins/vibe-keystone`, `source.ref: v0.2.1` → `v0.3.0` |
| Release gate | The tag must resolve on the remote AND the manifest must be fetchable at that tag, both verified via `gh api`, before the `ref` moves. A ref pinned to a nonexistent tag breaks installs for every stable-channel user. |

**Promotion is linear and non-negotiable:** ship on the solo repo, tag, verify, then bump the marketplace ref. Never edit both in parallel. That invariant is what makes "stable" mean anything.

## Architecture Overview

Keystone has no components in the software sense. Its architecture is an **instruction graph**: one entry-point skill that an agent reads on invocation, plus reference files it loads on demand.

```
                        ┌────────────────────────────┐
   /keystone  ─────────▶│  skills/keystone/SKILL.md  │  ~100 lines, always loaded
                        │  job · gate · guard · flow │
                        └────────────┬───────────────┘
                                     │ loads on demand
        ┌──────────────┬─────────────┼─────────────┬──────────────┐
        ▼              ▼             ▼             ▼              ▼
  derivability-  protected-      skeleton    progressive-    tenant-
    test.md       content.md       .md       disclosure.md  interview.md
        │              │             │             │              │
        └──────────────┴──────┬──────┴─────────────┘         repo-types.md
                              │                               capture.md
                              ▼
                   ┌──────────────────────┐
                   │  the produced file   │
                   │  <repo>/CLAUDE.md    │
                   └──────────────────────┘
```

### Data flow of one `/keystone` run

```
repo on disk
    │
    ├─▶ [1] INVENTORY ─────── git state, layout, manifests, existing CLAUDE.md,
    │                         .claude/, workflows, multi-surface signals
    │
    ├─▶ [2] INTERVIEW ─────── tenant identity, decision surface, persona, repo type
    │         │               inherited docs read TWICE: as source AND as exclusion list
    │         ▼
    ├─▶ [3] CLASSIFY ──────── every candidate line through the two-axis gate
    │         │                   ├── protected? ──▶ never cut, route by frequency
    │         │                   ├── derivable AND default-right? ──▶ CUT
    │         │                   └── otherwise ──▶ KEEP
    │         ▼
    ├─▶ [4] ROUTE ─────────── inline (needed every task)
    │                         nested keystone (location-gated, subtree-only work)
    │                         skill (invocable anywhere, situational)
    │                         docs/ pointer (deep reference)
    │         │
    │         ├──▶ nothing survives? ──▶ "NOT YET WORTH A KEYSTONE" verdict, stop
    │         ▼
    ├─▶ [5] DRAFT ─────────── seven sections, budget-aware
    │         ▼
    ├─▶ [6] SELF-CHECK ────── eight items; failures loop back to [3]
    │         ▼
    ├─▶ [7] WRITE ─────────── root CLAUDE.md only (diff + confirm if one exists)
    │                         everything else is PROPOSED
    │         ▼
    └─▶ [8] CAPTURE ───────── opt-in, default off, local-only, schema_version 2
```

## The gate

Implements `prd.md > Epic 1`. Lives in `references/derivability-test.md`.

### Two axes, both required to cut

| Axis | Question | Nature |
|---|---|---|
| **1. Derivability** | Could a session obtain this by running `ls`, reading a file, reading the manifest, or running `--help`? | **Mechanical.** Checkable. |
| **2. Default-correctness** | If this line were absent, would the model's default assumption be correct? | **Inferential.** A judgment call. |

**Cut requires YES on both.** Anything else stays.

**Tie-break:** uncertain on axis 2 means keep. Stated explicitly, because a gate that resolves ambiguity toward deletion will eventually delete something load-bearing.

The split between mechanical and inferential is stated in the reference rather than blurred. Blurring them lets a judgment call present itself as a mechanical check, which is how over-trimming gets justified.

### Provenance

The reference quotes `/doctor`'s criterion once, naming the binary version it was read from (`claude.exe` v2.1.220), so a reader knows axis 1 is Anthropic's rule and not a house preference. Axis 2 is sourced to the Fable field guide's under-specification passage.

## The guard

Implements `prd.md > Epic 2`. Lives in `references/protected-content.md`.

Three prongs. Prong 2 carries the `/prd` correction.

### Prong 1 — the gate has no authority here

Protected categories: persona, identity, voice rules, tone, banned phrases, taste, priorities, values, brand tokens, cultural reference material.

These fail `ls`/`cat`/`--help` by construction. That is the reason they are worth writing down, not a reason to cut them. Trimming them for length is a category error.

### Prong 2 — route by frequency, and the destination is not a free choice

| Content | Destination | Why |
|---|---|---|
| Needed on every task | **Inline** in the root keystone | Always-loaded is correct for always-needed |
| Situational, work happens anywhere | **A skill** | Skills are invocable from anywhere |
| Situational, work never leaves one subtree | **A nested keystone** | Location-gating is safe only when location is guaranteed |
| Never | *deleted to hit a budget* | — |

**Nested keystones and skills are not interchangeable.** Nested files are location-gated; they load only when work happens under that directory. Skills are invocable from anywhere. Marketing voice rules parked in a nested file under a marketing subtree fail silently the first time someone writes marketing copy from the repo root. **Default to a skill.** Reach for a nested keystone only when the work genuinely never leaves the subtree.

### Prong 3 — dedup points, never deletes the last copy

Before treating content as duplicated, establish that a canonical copy exists elsewhere. Running Keystone on the repo that *holds* the canonical voice guide must leave that guide in place. Dedup emits an inheritance pointer; it never emits a deletion of the only copy.

## The skeleton

Implements `prd.md > Epic 3`. Lives in `references/skeleton.md`.

| # | Section | Status | Spec |
|---|---|---|---|
| 1 | Orientation | ALWAYS | 3-5 lines. What the repo is for, its role among siblings. Not derivable: a repo cannot tell you it is the marketplace front door rather than a plugin. |
| 2 | Gotchas | ALWAYS | The center of gravity. **A gotcha is an unknown known** — an obvious-once-named detail, invisible until named. Each item names a failure that happened or would. |
| 3 | Non-standard conventions | CONDITIONAL | Divergences from a sane default only. Zero is valid and drops the heading. |
| 4 | Rationale | CONDITIONAL | Decisions an agent would otherwise undo. Folds into Gotchas when thin. |
| 5 | Pointers | ALWAYS | The progressive-disclosure spine. Skills, `docs/`, nested keystones, agents. |
| 6 | Decisions log | CONDITIONAL | Unchanged from v0.2.1. Follows the family decision-log-backend convention. |
| 7 | What NOT to do | CONDITIONAL | **Floor removed.** Zero is valid. |

**Removed as sections:** What's where, Tech Stack, Common tasks, Design system, Voice. Each survives only as residue that fails the gate, folded into Gotchas or Pointers. A path row survives when it carries rationale ("the load-bearing artifact is X"), not when it restates `ls`.

**The persona-inheritance blockquote survives unchanged**, all three cases. It is protected content, one line in output, and the model for how dedup should behave everywhere.

**Exemplars, not templates.** Two to four real lines per section, drawn from actual repos. The v0.2.1 skill used a dozen `{placeholder}` fill-in-the-blank blocks; those constrain output shape rather than communicating the criterion, and they are what this replaces.

### The declined verdict

When inventory and classification yield no genuine gotchas, no non-standard conventions, and no rationale, Keystone renders **"not yet worth a keystone"** rather than emitting headings with nothing under them.

- First-class outcome, not an error.
- Names what would make the repo worth one later.
- **Writes nothing.** Reports only. (Resolves `prd.md > Open Questions` item 4: the verdict is pure report. Writing a stub file would recreate the empty-skeleton problem the verdict exists to prevent.)
- Modeled on Vibe-Walk's "don't build a tour" verdict, which that cycle recorded as its differentiator.

## Progressive disclosure

Implements `prd.md > Epic 4`. Lives in `references/progressive-disclosure.md`.

### The loading model (verified)

| Surface | When it loads |
|---|---|
| Root `CLAUDE.md` | Always |
| `.claude/CLAUDE.md` | Always |
| Nested `<dir>/CLAUDE.md` | Only when working under that directory |
| `.claude/rules/*.md` | Always |

Source: the `/doctor` prompt in `claude.exe` v2.1.220 — *"the root file and `.claude/CLAUDE.md` (always loaded), nested-directory CLAUDE.md files (loaded when working under that directory), and `.claude/rules/*.md`."*

**Always-loaded files matter most.** The budget tightens at the root and relaxes for nested files.

### Multi-surface detection

Runs during inventory. Signals: workspace manifests (`pnpm-workspace.yaml`, `lerna.json`, a `packages/` tree), multiple app roots, distinct deploy targets, more than one language toolchain at top level.

On detection: propose a thin root plus per-surface nested files. **Proposed, never auto-written.**

**The root-stays rule:** anything an agent must see on every task stays at the root regardless of which surface it describes. This is the mitigation for knowledge fragmentation.

### The budget

~50 lines target, ~100 ceiling, for the root file.

**The ceiling is soft.** A repo with twenty genuine root-level gotchas correctly produces a file over 100 lines. **A gotcha is never cut to hit budget.** The budget squeezes derivable content only; it is a forcing function for the gate, not a cap on real content. A 40-line file full of derivable content still fails.

## Keystone's own SKILL

Implements `prd.md > Epic 5`.

`SKILL.md` at ~100 lines (ceiling 130):

1. Frontmatter — `name: keystone` unchanged; `description` rewritten, **unquoted** (escaped inner quotes blank the in-session listing; fixed in `4e8bea6`, must not regress). Trigger phrases preserved.
2. The job, 3-4 lines.
3. The gate in brief, 3 lines → `references/derivability-test.md`.
4. The guard in brief, 3 lines → `references/protected-content.md`.
5. The flow — inventory, interview, draft, self-check, propose — each 2-5 lines with a reference pointer.
6. The eight-item self-check, **inline** (needed every run).
7. Capture, 2 lines → `references/capture.md`.
8. What you do not do — carried from v0.2.1 with the voice-rules item replaced by the guard, plus a new `/doctor` boundary item.

An agent that reads only `SKILL.md` and no references must still behave correctly. The references carry depth, not correctness.

### The eight-item self-check

- [ ] Every line passes the gate, or is protected content
- [ ] Protected content was relocated, never deleted, and pointers resolve
- [ ] Nothing restates the inherited global or tenant file, unless this repo is its canonical home
- [ ] Root file is within budget, or the overflow has a named destination
- [ ] Gotchas is non-empty and each item names a real failure mode
- [ ] Multi-surface repos got a nested-keystone proposal
- [ ] No snapshot lists that rot
- [ ] Every referenced path exists on disk

## Data Model

Two persisted shapes. Both local, both optional.

### `captures.jsonl` — schema_version 2

`~/.claude/plugins/data/vibe-keystone/captures.jsonl`. One JSON object per line, appended. Opt-in per run, default off.

```json
{
  "schema_version": 2,
  "timestamp": "<ISO local datetime>",
  "run_type": "fresh | refresh | declined",
  "tenant_kind": "626labs | other-org | individual",
  "repo_type_autodetected": "code | marketing-content | long-form-writing | infra-mixed",
  "repo_type_final": "code | marketing-content | long-form-writing | infra-mixed",
  "sections_included": ["orientation", "gotchas", "pointers"],
  "sections_dropped": ["non-standard-conventions", "rationale", "decisions-log", "what-not-to-do"],
  "sections_overridden": [{ "section": "persona", "from_default": "inherit", "to": "override" }],
  "sections_requested_not_in_skeleton": ["<free-text>"],
  "nested_proposed": 0,
  "skills_proposed": 0,
  "root_line_count": 47
}
```

Changes from v1: `schema_version` 1→2; section vocabulary is the seven new names; `run_type` gains `declined`; three new fields (`nested_proposed`, `skills_proposed`, `root_line_count`) so `evolve-keystone` can see whether progressive disclosure is actually being used and whether the budget holds in practice.

**Privacy rules carry forward verbatim and are non-negotiable:** never the tenant name, repo name, repo file paths, source code, or any CLAUDE.md content. Opt-in per run. Local only, no network, ever. A failed append never blocks the run.

### `~/.claude/profiles/builder.json`

Read-only for Keystone. Untouched by this cycle.

## File Structure

```
vibe-Keystone/
├── .claude-plugin/
│   └── marketplace.json                    # canary self-listing
├── plugins/vibe-keystone/
│   ├── .claude-plugin/
│   │   └── plugin.json                     # MODIFIED: 0.2.1 → 0.3.0, description rewritten
│   └── skills/
│       ├── keystone/
│       │   ├── SKILL.md                    # REWRITTEN: 390 → ~100 lines
│       │   └── references/                 # NEW — the progressive-disclosure tree
│       │       ├── derivability-test.md    # NEW ~60  the two-axis gate + worked examples
│       │       ├── protected-content.md    # NEW ~50  three prongs
│       │       ├── skeleton.md             # NEW ~90  seven sections + declined verdict
│       │       ├── progressive-disclosure.md # NEW ~75  nested vs skills, budget
│       │       ├── tenant-interview.md     # NEW ~70  interview + exclusion-list use
│       │       ├── repo-types.md           # NEW ~45  reframed around gotcha shape
│       │       └── capture.md              # NEW ~50  opt-in capture, schema v2
│       └── evolve-keystone/
│           └── SKILL.md                    # MODIFIED: v1/v2 comparability, new skeleton path
├── docs/                                   # NEW — Cart cycle #18 artifacts
│   ├── builder-profile.md
│   ├── scope.md
│   ├── prd.md
│   ├── spec.md
│   ├── checklist.md                        # written by /checklist
│   ├── reflection.md                       # written by /reflect
│   └── v0.3-migration-friction.md          # written during /build Phase 2
├── assets/brand/                           # unchanged
├── process-notes.md                        # NEW
├── proposed-changes-harness.md             # MODIFIED: status header, proposals absorbed
├── CHANGELOG.md                            # MODIFIED: v0.3.0 entry
├── README.md                               # MODIFIED: lines 20-32, 38-43, 53-69
├── PRIVACY.md                              # UNCHANGED — the zero-scripts invariant holds
└── LICENSE                                 # unchanged
```

**Cross-repo, written during validation and ship:**

```
vibe-plugins/CLAUDE.md                      # migrated (smoke test, first)
Project-626Labs-1/CLAUDE.md                 # migrated + nested proposals
Celestia3/CLAUDE.md                         # migrated
vibe-cartographer/CLAUDE.md                 # migrated (check for a generator first)
Projects/CLAUDE.md                          # migrated (heavy protected content)
vibe-plugins/.claude-plugin/marketplace.json # ref bump, last
vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md  # back-merge gate v2 + /prd corrections
```

## Key Technical Decisions

### 1. The `/doctor` boundary is regenerate-versus-trim, not births-versus-maintains

Resolves `prd.md > Open Questions` item 1, which was flagged as needing an answer before `/spec`.

"Keystone births, `/doctor` maintains" does not survive contact with the refresh case: running `/keystone` on a repo that already has a CLAUDE.md is maintenance by any plain reading, and the five migrations in this cycle are exactly that.

The distinction that actually holds is the **input**:

| | Keystone | `/doctor` |
|---|---|---|
| **Input** | The repo | The existing file |
| **Operation** | Regenerate — re-derive a candidate, diff it, confirm, write | Trim — propose deletions of derivable content, edit in place |
| **Can add content?** | Yes. Discovers gotchas from the repo that were never in the old file. | No. Only removes and relocates what is already there. |
| **Granularity** | Whole file | Line level |

Keystone can find a trap nobody wrote down, because it reads the repo. `/doctor` cannot, because it reads the file. That is a real capability difference, and it means both tools have a job on an existing CLAUDE.md without either being redundant.

**Tradeoff accepted:** a user could reasonably reach for either on a fat existing file, and we do not stop them. The skill states the distinction and points at `/doctor` for in-place trimming; it does not try to detect and redirect. Over-engineering a boundary users can navigate themselves.

### 2. The gate is two-axis, and one axis is admittedly soft

Axis 1 is mechanical and checkable. Axis 2 is a judgment call. A cleaner design would have used only the mechanical axis.

**Tradeoff accepted:** the purely mechanical version is wrong. Verified against a real line — "the load-bearing artifact is `.claude-plugin/marketplace.json`" is derivable in principle and would have been cut, while the industry-default assumption (the README or `package.json` is the center of a repo) is wrong. Single-axis was going to trim load-bearing content out of every keystone the tool shipped. A soft second axis that keeps the right content beats a crisp single axis that cuts it. The softness is disclosed and the tie-break is stated rather than left to the reader.

### 3. No executable validator, again

Path existence and placeholder leakage are mechanical checks that would be better as code, and the parked harness proposal #1 specs them.

**Tradeoff accepted:** `PRIVACY.md` promises no scripts, zero outbound, fully determined by SKILL files. Users chose the plugin partly on that. Shipping a Node linter to save some agent-side checking is a bad trade against an invariant. The checks stay in the self-check list and run agent-side. Revisit only if the invariant itself is ever reconsidered deliberately.

## Dependencies & External Services

None. No APIs, no databases, no hosting, no keys, no rate limits, no network calls at any point in a Keystone run.

The only external touch in the whole cycle is `gh api` during release verification, using ambient CLI auth, and that is the ship procedure rather than the product.

## Open Issues

Surfaced during architecture self-review.

1. **The `references/` tree is always-loaded in one sense.** Skills load their `SKILL.md` when triggered; the references load on demand. But `SKILL.md` at ~100 lines is itself always-loaded once the skill fires. The restructure moves cost from "every keystone run" to "runs that need the depth", which is the win — it does not make the skill free. Worth stating honestly in `/reflect` rather than claiming a larger reduction than was achieved.

2. **Axis 2 has no worked-example coverage problem yet.** The worked-example table must include at least one derivable-but-keep row, or readers will calibrate on cuts alone and reconstruct the single-axis rule by induction. Called out in `prd.md > Epic 1` acceptance criteria; flagged here because it is the kind of requirement that gets dropped when a table is being trimmed for length.

3. **No test exists for "an agent reading only SKILL.md behaves correctly."** It is an acceptance criterion in Epic 5 with no mechanical check behind it. The honest verification is the estate migration: if the migrations required repeatedly opening references that `SKILL.md` should have summarized, the summary is too thin. Measured at `/reflect` from the friction log.

4. **Path rot after write.** The self-check verifies referenced paths at write time only. Nothing verifies them later. That is the deferred drift doctor. The skill should name the limitation rather than implying freshness is guaranteed. Carried from `prd.md > Open Questions` item 3, unchanged.

5. **A bloated inherited global file propagates.** Dedup points at the global file and inherits whatever is wrong with it. Resolution stands from the PRD: note it in run output, do not fix it. The global file is `/doctor`'s territory.
