# Privacy Policy — Vibe Keystone

**Effective:** 2026-04-28
**Plugin:** vibe-keystone
**Author:** 626Labs LLC

## Summary

Vibe Keystone collects no telemetry, runs no analytics, and transmits no
personal data. Everything the plugin produces lives on your local
machine.

## What the plugin reads and writes

All on your local filesystem:

- **Your project's source files** — Keystone reads the structure of the
  repository you run it against (file inventory, framework signals,
  language signals) to classify the repo type and adapt the produced
  CLAUDE.md to the right shape (code platform / marketing site /
  long-form writing / mixed). Read-only on source files.
- **The repo's existing `CLAUDE.md`** — if one is present and stale,
  Keystone reads it for context before producing the refresh. Otherwise
  starts blank.
- **A new `CLAUDE.md` at the repo root** — the plugin's primary output.
  Written into the project directory you ran the keystone command on.
  Local only.
- `~/.claude/profiles/builder.json` — the unified builder profile (shared
  with other 626Labs plugins). Keystone reads it to honor your persona,
  voice rules, and decision-surface preferences during the interview.
  Read-only.

## What the plugin transmits

**Nothing.** Vibe Keystone makes no outbound network calls. There is no
update-notifier, no telemetry endpoint, no remote configuration fetch.
The plugin runs entirely within the Claude Code / plugin host process
on your local machine.

## What the plugin does NOT do

- No telemetry, no usage analytics, no install counters, no first-run
  pings.
- No personal data collection of any kind.
- No third-party sharing, advertising, or tracking.
- No remote configuration — the plugin's behavior is fully determined by
  the SKILL files shipped in the release you installed.
- No exfiltration of your project contents. Repo inventory and CLAUDE.md
  generation run entirely in the local plugin host process.

## What about the LLM that calls these skills?

Keystone's skill is loaded by Claude Code (or a compatible plugin host).
The host sends parts of the skill instructions and the conversation
context to Anthropic's API to drive the agent. **That traffic is
governed by Anthropic's privacy policy, not this one.** Keystone itself
does not send any data to Anthropic or anywhere else; the plugin host is
the entity making those API calls on your behalf.

## Contact

Questions or concerns: open an issue at
[github.com/estevanhernandez-stack-ed/vibe-Keystone/issues](https://github.com/estevanhernandez-stack-ed/vibe-Keystone/issues).

## Changes

If we ever change what the plugin reads, writes, or transmits, this file
will be updated and a corresponding CHANGELOG entry will reference the
change. Versioned by the same release tag as the plugin.
