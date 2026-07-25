<!-- markdownlint-disable MD024 -->
<!-- Keep-a-Changelog uses duplicate "Added / Changed / Fixed" headings per version by design. -->

# Changelog

All notable changes to Vibe Keystone are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.3.0] — 2026-07-25 — The derivability rewrite

**Breaking: generated keystones change shape.** The skeleton was calibrated for a model generation that needed to be told what it could already see. Claude Code's own `/doctor` now names three things to cut from a CLAUDE.md — directory layouts, tech-stack lists, architecture overviews — and those were three of Keystone's ALWAYS sections. The three it says to keep, gotchas and rationale and non-standard conventions, had no section at all.

### Changed

- **The generating question.** Not "which sections does this repo need" but "what would a competent agent get wrong after reading this repo?"
- **The gate is two-axis.** Cut requires *both* that a session could derive the line (`ls`, `cat`, the manifest, `--help`) *and* that the model's default assumption would be right without it. Axis 1 is mechanical and matches `/doctor`'s criterion, read from `claude.exe` v2.1.220. Axis 2 comes from the Fable field guide's under-specification failure — thin context does not produce a model that asks, it produces one that substitutes an industry default. Uncertain on axis 2 means keep. A single-axis gate would have cut "the load-bearing artifact is `marketplace.json`" from every keystone it touched.
- **Seven sections replace ten:** Orientation, Gotchas, Non-standard conventions, Rationale, Pointers, Decisions log, What NOT to do. **What's where, Tech Stack, Common tasks, Design system, and Voice are no longer sections** — each survives only as residue that fails the gate.
- **The 3-item floor on "What NOT to do" is removed.** Zero is valid. A floor manufactures filler, and "don't commit secrets" is the filler it manufactured.
- **A gotcha has a definition:** an *unknown known*, obvious once named and invisible until then. If you cannot name the failure mode, it is not a gotcha.
- **Keystone's own SKILL: 390 lines to 82**, plus an eight-file `references/` tree. Always-loaded surface down 79 percent; total content is larger, and the win is that depth now loads only when needed. The dozen fill-in-the-blank template blocks are gone — criterion plus one real exemplar replaces them.
- **The tenant interview reads inherited docs twice**, as a source and as an exclusion list. A repo keystone that repeats the global file creates two copies that drift.
- **Repo types no longer map to sections.** The gate decides sections; types tell you where to look for gotchas.

### Added

- **The protected-content guard.** Persona, voice, taste, priorities, and brand fail the gate by construction, which is exactly why they are worth writing down. Three prongs: the gate has no authority over human-supplied context; protected content routes by frequency of need and is never deleted for length; dedup points at the canonical copy and never deletes the last one. Inside a persona, **identity is not procedure** — a long persona is often a short identity carrying a lot of restated process.
- **Nested keystones and skill extraction** as first-class destinations, with the loading model verified rather than assumed. Nested files are location-gated; skills are invocable anywhere; they are not interchangeable.
- **A line budget** — ~50 target, ~100 ceiling for a root file. Explicitly soft: a gotcha is never cut to hit it, and a 40-line file of derivable content still fails.
- **The declined verdict.** When a repo yields no gotchas, conventions, or rationale, Keystone says so and writes nothing rather than emitting empty headings.
- **`references/file-ownership.md`.** An existing CLAUDE.md may be generated or tool-owned; a diff against a generated file looks completely normal and the work vanishes on the next render. Covers generated-file detection, tool-owned marker regions (matched on lines that *are* the marker, never by substring), and the colonized case where every owner is something else.
- **A shape-change warning.** When an existing `CLAUDE.md` carries sections the new skeleton no longer produces, the builder is told **before** the diff rather than left to interpret it. A diff that deletes What's where, Tech Stack, and Common tasks looks like the tool malfunctioning; it is a deliberate shape change, and saying so is the difference between an informed yes and a confused no. Anchored to the sections actually present, never to a claim about which version wrote them — a hand-authored file can carry those headings too. Detected from the file itself, so it costs no state and no new write surface.
- **Capture at `schema_version: 2`** — seven-name section vocabulary, `run_type: "declined"`, plus `nested_proposed` / `skills_proposed` / `root_line_count` so `evolve-keystone` can see whether progressive disclosure gets used and whether the budget holds.

### Fixed

- **`evolve-keystone`** retargeted at the reference tree, and taught that v1 and v2 captures use different section vocabularies and must never be pooled when aggregating section rates.

### Validated on

Five real keystones across the estate, which found five defects that would have shipped:

| Repo | Before | After |
|---|---|---|
| `vibe-plugins` | 147 | 46 |
| `Project-626Labs-1` | 302 | dry run, 191-line template → 71 |
| `Celestia3` | 205 | 140 (authored; it had no keystone at all) |
| `vibe-cartographer` | 204 | 152 (hand-authored 103 → 51) |
| `Projects` | 90 | 52 |

The dogfood found: Keystone would silently overwrite generated CLAUDE.md files; a naive surface sweep proposes nested keystones into git worktrees; marker regions identified by substring destroy the block they meant to protect; a persona's restated procedure needed splitting from its identity; and a repo can have a long CLAUDE.md containing nothing about itself.

It also proved the `/doctor` boundary empirically. Classifying `Project-626Labs-1` surfaced that two different VS Code extensions live in that repo — a fact about the repo, absent from the file, that `/doctor` could not have found. Keystone regenerates from the repo and can add; `/doctor` trims the file and can only subtract.

Full record: [`docs/v0.3-migration-friction.md`](docs/v0.3-migration-friction.md).

## [0.2.1] — 2026-05-23 — decision-log MCP rename + generic generated framing

Patch release. Fixes a stale MCP server reference in the keystone skill — including the template it writes into other people's CLAUDE.md files.

### Fixed

- **Stale decision-log MCP reference (3 occurrences) in the keystone skill.** The 626Labs MCP server was renamed `626Labs` → `626labs-cloud`; the skill still referenced the old `mcp__626Labs__manage_decisions` name in the Step 1 interview options, the adaptation table, and — most importantly — the **decisions-log template emitted into generated CLAUDE.md files**. Corrected all three to `mcp__626labs-cloud__manage_decisions` and reframed the generated guidance to be generic: a decision-log MCP is optional and auto-detected (the 626Labs dashboard is the recognized instance, not a hard dependency baked into every user's keystone), with a named fallback (`decisions.md` / tracker / skip) when no such MCP is present. Keeps Keystone tenant-neutral — it generates structure for other people's repos, so its MCP guidance must not hardcode 626Labs into every produced file. See [`plugins/vibe-keystone/skills/keystone/SKILL.md`](plugins/vibe-keystone/skills/keystone/SKILL.md).

## [0.2.0] — 2026-05-23 — The capture + reflect loop

The smallest evolution loop that fits a one-shot generator: an opt-in structural sensor on each run (Tier 0) plus a reflective skill that mines the captures for skeleton improvements (Tier 1).

### Added

- **Tier 0 — opt-in evolution capture.** A new Step 6 in the keystone skill offers, at the end of each run, to record a small anonymous structural note (repo type, and which skeleton sections were included / dropped / overridden) to `~/.claude/plugins/data/vibe-keystone/captures.jsonl`. Opt-in, off by default, local-only, agent-appended (no script shipped). Captures structure, never code or identity.
- **Tier 1 — `/vibe-keystone:evolve-keystone`.** A new reflective skill that reads the capture log, finds patterns (sections always dropped, defaults always overridden, repo-types the classifier keeps getting wrong, sections users keep asking for), and proposes concrete skeleton/classifier edits to `proposed-changes.md`. Never auto-applies; suppressed below 5 captures. Mirrors the `vibe-cartographer:evolve` never-auto-apply discipline.

### Changed

- **`PRIVACY.md`** updated to disclose the opt-in capture file as the one place Keystone writes outside the repo's `CLAUDE.md`. Zero-network and zero-telemetry guarantees are unchanged — the capture is local-only and opt-in. Effective date bumped to 2026-05-23.

### Notes

- Together these add the smallest evolution loop that fits a one-shot generator — capture (Tier 0) + reflect (Tier 1) — without cargo-culting the full session / friction / decay stack from Cartographer. A drift-doctor over already-shipped CLAUDE.md files remains deliberately out of scope.

## [0.1.1] — 2026-04-28 — Submission-readiness polish

Patch release. Metadata-only. No behavioral change.

### Added

- **`PRIVACY.md`** at the repo root — explicit "no telemetry, no analytics, no third-party sharing" statement plus the full read/write/transmit surface (repo structure, existing CLAUDE.md, the new CLAUDE.md output, `~/.claude/profiles/builder.json`). Documents Keystone's zero-outbound-network design.
- **`CHANGELOG.md`** at the repo root — Keep-a-Changelog format.
- **`plugin.json` metadata fields** required for marketplace discovery: `repository`, `license`, `keywords`, and `author.url`. `homepage` was already present. Brings the manifest in line with the [official Claude Code plugin schema](https://code.claude.com/docs/en/plugins-reference#plugin-manifest-schema) so the plugin shows up cleanly in marketplace searches and discovery views.

### Notes

- Submission-readiness pass for the official Claude Code marketplace at [claude.ai/settings/plugins/submit](https://claude.ai/settings/plugins/submit). v0.1.1 is the tag the submission references.

## [0.1.0] — 2026-04-28 — Initial release

Initial release of Vibe Keystone — the structural-foundation plugin for the 626Labs Vibe ecosystem. Bootstraps a CLAUDE.md (the load-bearing structural file every agent decision in a repo rests on) with tenant-aware adaptation.

### Added

- **`/keystone` command** — interviews the user for org, decision surface, voice rules, and persona before drafting, so the produced CLAUDE.md reflects YOUR conventions, not 626Labs defaults baked in.
- **Tenant-aware drafting.** The skill detects repo type (code platform / marketing site / long-form writing / mixed) from filesystem signals and adapts the produced CLAUDE.md's shape accordingly.
- **Plugin marketplace structure** — `.claude-plugin/marketplace.json` at repo root so the repo is installable as a single-plugin marketplace via `/plugin marketplace add estevanhernandez-stack-ed/vibe-Keystone`.
- **MIT License** at repo root.
- Repo restructured to match other 626Labs solo-repo conventions: plugin under `plugins/vibe-keystone/`; skills under `plugins/vibe-keystone/skills/keystone/SKILL.md`.
- Available in the [`vibe-plugins`](https://github.com/estevanhernandez-stack-ed/vibe-plugins) aggregated marketplace as the seventh plugin (foundations layer alongside Thesis Engine).
