# Progressive disclosure

Content that fails the gate gets cut. Content that passes but is not needed on every task gets **routed**, not deleted.

## The loading model

Read from `claude.exe` v2.1.220, not inferred:

| Surface | When it loads |
|---|---|
| Root `CLAUDE.md` | Always |
| `.claude/CLAUDE.md` | Always |
| `<dir>/CLAUDE.md` | **Only while working under that directory** |
| `.claude/rules/*.md` | Always |

Always-loaded files are the expensive ones. The budget tightens at the root and relaxes for nested files.

## Where content goes

| Destination | Use when | Reachable |
|---|---|---|
| **Inline** at the root | Needed on every task | Always |
| **A skill** | Situational, and the work can start anywhere | From anywhere, by invocation |
| **A nested keystone** | Situational, and the work never leaves one subtree | Only under that directory |
| **A `docs/` pointer** | Deep reference material, read rarely and deliberately | On demand |
| **A derivation command** | Genuinely derivable, but tedious enough that people want it written down | On demand, always current |
| **A pointer to an inherited file** | Already stated in a global or tenant file | Always, via that file |

**Skills and nested keystones are not interchangeable.** See [`protected-content.md`](protected-content.md) prong 2 — a nested file is location-gated, so parking situational rules there fails silently when the work starts somewhere else. Default to the skill.

### The derivation-command destination

When something is derivable but people keep writing it down anyway, the answer is usually to ship the derivation instead of the output.

A marketplace repo's keystone carried a fifteen-row table of every plugin and its source repo. Derivable, so the gate cuts it — but it was there because people genuinely wanted the roster. The file already contained the fix: a one-line command that regenerates the table from the manifest, plus the rule that the manifest wins on disagreement. Keep the command and the rule; drop the table. The output goes stale silently; the command never does.

Reach for this when the cut would remove something useful rather than something dead.

## The root-stays rule

**Anything an agent must see on every task stays at the root**, regardless of which surface it describes.

This is the mitigation for fragmentation. A rule split into a subtree is a rule that stops existing the moment work happens elsewhere, and nothing reports the absence.

## Multi-surface repos

Detect during inventory. Signals:

- A workspace manifest (`pnpm-workspace.yaml`, `lerna.json`, a `packages/` tree)
- More than one app root with its own entry point
- Distinct deploy targets
- More than one language toolchain at the top level

When detected, propose a thin root plus per-surface nested keystones. **Verify the surfaces against the actual tree before proposing them** — infer boundaries from what is there, not from what the repo's name suggests.

**Propose, never auto-write.** Only the root `CLAUDE.md` is written. Nested files, skills, agents, rules, and hooks are proposals the builder accepts or declines.

## The budget

**~50 lines target. ~100 ceiling.** For the root file.

**The ceiling is soft.** A repo with twenty genuine root-level gotchas correctly produces a file over 100 lines. That is the system working.

**Never cut a gotcha to hit the budget.** The budget squeezes derivable content and nothing else. If the file is long and every line passes the gate, the file is the right length.

The budget is a forcing function for the gate, not an independent rule. **A 40-line file full of derivable content still fails.** Hitting the number proves nothing on its own.

When a section genuinely overflows, route it per the table above. Overflow relocates. It does not get deleted.
