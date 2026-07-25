# Protected content

[`derivability-test.md`](derivability-test.md) governs facts that live in the codebase. It has no authority over content that does not.

### Prong 1 — the test does not apply

Protected categories:

- Persona and identity
- Voice rules, tone, banned phrases
- Taste: "we don't do it that way here"
- Priorities and values
- Brand tokens
- Cultural reference material

None of it is in the repo. It lives in a person's head or a style doc, so it fails `ls`, `cat`, and `--help` by construction. **That is precisely why it is worth writing down** — it is the one class of content an agent cannot reconstruct from the codebase.

Trimming it because a file is long is a category error. You are not removing redundancy; you are removing the only copy of something the repo cannot regenerate.

### Prong 2 — protected does not mean pinned inline

Route by **how often it is needed**, never by how long it is.

| Content | Destination |
|---|---|
| Needed on every task in the repo | Inline in the root keystone |
| Situational, and the work can happen anywhere | **A skill** |
| Situational, and the work never leaves one subtree | A nested keystone in that subtree |
| Too long | *Not a category. Length is not a routing signal.* |

**Skills and nested keystones are not interchangeable.** A nested `CLAUDE.md` is **location-gated**: it loads only while work is happening under that directory. A skill is **invocable from anywhere**. Default to the skill; reach for a nested keystone only when the work genuinely never leaves the subtree.

**Worked example.** A repo's marketing voice rules are non-derivable and only needed when writing marketing copy. Parking them in `marketing/CLAUDE.md` looks right and fails silently the first time someone drafts a landing page from the repo root — the file never loads, the rules never apply, and nothing reports an error. The same rules as a `marketing-voice` skill, with a one-line pointer in the keystone, work from anywhere.

Nothing here is ever deleted to hit a budget. It relocates, and it leaves a pointer.

### Prong 3 — dedup points, it never deletes the last copy

When a repo keystone restates content from a global or tenant file, the fix is a one-line inheritance pointer, not deletion.

Before treating anything as duplicated, establish that a canonical copy exists **elsewhere**.

**The failure this prevents:** run Keystone in the repo that *holds* the canonical voice guide, and naive dedup reads the voice rules as "already covered by the voice guide" — which is the same file. It deletes the source and points at nothing. Check whether this repo is the canonical home before calling anything a duplicate.

The persona-inheritance blockquote is the model for correct behavior: one line, points at the canonical definition, restates nothing.
