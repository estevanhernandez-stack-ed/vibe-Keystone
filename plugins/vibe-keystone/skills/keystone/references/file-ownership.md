# File ownership

An existing `CLAUDE.md` is not always hand-authored. Writing over a generated one produces work that disappears on the next render, silently, with no error.

Check before drafting:

- **A generated-file header.** "GENERATED FILE — do not hand-edit", a named source template, a render command.
- **A sibling template** — `CLAUDE.template.md`, `CLAUDE.md.tmpl`, or similar.
- **Interpolation syntax** in a sibling — `{{...}}`, `${...}` — plus a resolver script and a manifest.
- **A render script** in `package.json` or a Makefile: `vars:resolve`, `docs:render`, `claude:build`.
- **Tool-injected regions** delimited by markers such as `<!-- toolname:start -->` / `<!-- toolname:end -->`. These are owned by that tool and re-injected after it next runs, often from a commit hook.

When the file is generated:

- **Retarget.** The template is the file to change, not the output. Regenerate afterward with the project's own command.
- **Leave tool-owned regions alone.** They are that tool's contract, not yours. Cutting a block a hook re-injects makes the diff churn forever.
- **Say so.** Tell the builder the file is generated, name the source and the render command, and let them decide before anything is written.

A file can have more than one owner. Establish the ownership map before the first edit.

**When every owner is something else, the repo has no keystone.** A `CLAUDE.md` can be long, current, and contain nothing about the project — tool-injected blocks plus a ported ruleset for some other plugin, and not one line of orientation, gotchas, or conventions.

Do not "rightsize" that file. There is nothing to trim: cutting a tool-owned block gets it re-injected, and cutting a deliberately-placed ruleset deletes someone's work. Say what is actually true:

> This repo's `CLAUDE.md` is fully occupied by other content — {name the owners}. None of it describes the project. What you have is not a thin keystone, it is an absent one. Writing a real keystone here is new work, not a trim.

Then offer to write one, and treat it as authoring rather than migration. The git log is the best seed: shipped bug fixes are gotchas that already cost someone something.

## Identifying a tool-owned region

Match the marker on a **line of its own** — `line.strip() == "<!-- tool:start -->"` — never by substring search, and assert the extracted region's size before using it.

Substring search fails on exactly the files that deserve care. A repo disciplined enough to warn "don't hand-edit between these markers" has now written the marker into its prose, so the first match is a sentence, not the block. Splicing on that match silently deletes the real region.

This is not hypothetical. It happened during the v0.3 migration: a 101-line tool block was replaced by a fragment of the sentence documenting it, and only a post-write line-count check caught it.

**After any splice around a tool-owned region, verify:** the region is still present, its line count is unchanged, and the file's total line count moved by the amount you intended.
