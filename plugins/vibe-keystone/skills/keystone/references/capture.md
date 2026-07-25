# Capture

Keystone is a one-shot generator. It writes a file and never sees it again, so on its own it cannot learn which parts of the skeleton it keeps getting wrong. Capture is the smallest sensor that fixes that, and it is **strictly opt-in**.

## Ask once, after the file lands

> "Want me to record a small, anonymous note about what this run produced, so `/vibe-keystone:evolve-keystone` can spot patterns and improve the skeleton over time? Local-only, opt-in, captures structure — never your code or your org's name. [y/N]"

**Default is no.** A "no", or no answer, writes nothing.

## Only on yes

Append one JSON line to `~/.claude/plugins/data/vibe-keystone/captures.jsonl`. Create the directory if absent. The agent appends directly — **Keystone ships no scripts**, and this does not introduce one.

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

**Section vocabulary** is the seven skeleton names: `orientation`, `gotchas`, `non-standard-conventions`, `rationale`, `pointers`, `decisions-log`, `what-not-to-do`.

`repo_type_autodetected` is the inventory's classification; `repo_type_final` is what it became after the interview. When they differ, that is the signal `evolve-keystone` uses to tune the classifier.

`run_type: "declined"` records a "not yet worth a keystone" verdict. Those runs are signal too — a classifier that declines too often, or never, is worth knowing about.

`nested_proposed`, `skills_proposed`, and `root_line_count` show whether progressive disclosure is actually being used and whether the budget holds on real repos.

## Schema history

**v1 → v2.** v1 used the ten-section vocabulary from the pre-v0.3 skeleton. Those names no longer exist. `evolve-keystone` must not pool v1 and v2 entries when aggregating section names; classifier-miss and requested-but-missing aggregation stay valid across both.

## Hard privacy rules

- **Never** write the tenant's name, the repo's name, file paths from the repo, source code, or any CLAUDE.md content. Only the structural signal above.
- **Opt-in per run.** Default off.
- **Local only. No network, ever.** This is the one place Keystone writes outside the target repo's `CLAUDE.md`, and it is disclosed in `PRIVACY.md`.
- If the append fails for any reason, say so in one line and move on. Capture never blocks a run.
