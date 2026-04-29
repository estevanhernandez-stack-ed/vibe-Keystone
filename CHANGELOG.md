<!-- markdownlint-disable MD024 -->
<!-- Keep-a-Changelog uses duplicate "Added / Changed / Fixed" headings per version by design. -->

# Changelog

All notable changes to Vibe Keystone are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
