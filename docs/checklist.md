# Build Checklist

## Build Preferences

- **Build mode:** Autonomous. Experienced builder, `mode: builder`, `autonomy_level: fully-autonomous`. Flow through; do not re-ask what the record answers.
- **Comprehension checks:** N/A (autonomous mode).
- **Git:** Commit after each item. Conventional commits. Cross-repo work always uses `git -C <absolute-path>` — stuck-cwd is this estate's documented failure mode and five of these items write outside `vibe-Keystone`.
- **Verification:** Yes. The checkpoints are the three **dogfood gates** (items 4, 6, 9). Additionally, and overriding autonomy: **every estate migration shows a diff and gets Este's confirmation before the file is written.** Keystone's own rule is never to overwrite an existing CLAUDE.md without a diff and a confirm, and this cycle must honor the rule it is validating. Five live repos he owns; autonomy does not extend to silently rewriting them.
- **Check-in cadence:** N/A (autonomous mode).

### Sequencing note — the flaw this ordering fixes

The inbound plan built all seven references plus the SKILL rewrite, then migrated five real files. That is integrate-at-end, and the builder profile records it failing three times: lesson (bbb) *dogfood earlier*, lesson (ccc) *wire-as-you-build, not integrate-at-end*, and *structural-green != works* — logged as "now a law" after the Vibe-Lingual cycle, where a full end-to-end dogfood caught three P0s that 245 passing unit tests missed.

This checklist interleaves instead. The first real migration runs at item 4, immediately after the gate, guard, and skeleton exist and before anything else is written. Two more gates follow. A defect found at a gate stops the run and fixes the reference tree before the next item, rather than propagating through four more files.

**13 items** rather than the 8-12 guideline. The overage is the three dogfood gates, which are the point.

## Checklist

- [ ] **1. The two-axis gate**
  Spec ref: `spec.md > The gate`
  What to build: Create `plugins/vibe-keystone/skills/keystone/references/derivability-test.md` (~60 lines). State the gate as two questions, both required to cut: (a) derivability — could a session get this from `ls`, `cat`, the manifest, or `--help`? (b) default-correctness — if absent, would the model's default assumption be right? Label axis 1 mechanical and axis 2 inferential; state the tie-break (uncertain on axis 2 means keep). Quote `/doctor`'s criterion once, naming `claude.exe` v2.1.220 as the source. Define the three keep-categories. Include the worked-example table with at least seven rows, **at least one of which is derivable-but-keep** so readers do not induct the single-axis rule from a table of pure cuts. Cross-link to `protected-content.md`.
  Acceptance: `prd.md > Epic 1` criteria. File under 70 lines. Contains at least one derivable-but-keep row. Contains a `protected-content.md` cross-link.
  Verify: `wc -l` under 70; `grep -c "protected-content.md"` at least 1; read the worked table and confirm a keep-verdict row exists whose axis-1 answer is "yes, derivable".

- [ ] **2. The protected-content guard**
  Spec ref: `spec.md > The guard`
  What to build: Create `references/protected-content.md` (~50 lines). Three prongs. Prong 1: the gate has no authority over human-supplied context; enumerate the categories; state the reasoning (these fail `ls`/`cat`/`--help` by construction, which is why they are worth writing down). Prong 2: route by frequency, with the destination table — always-needed stays inline, situational-anywhere goes to a **skill**, situational-subtree-only goes to a nested keystone, nothing is ever deleted for length. State plainly that nested keystones are location-gated and skills are invocable anywhere, so they are not interchangeable; include the marketing-voice-rules failure as the worked example. Prong 3: dedup points, never deletes the last copy; include the canonical-voice-guide failure case.
  Acceptance: `prd.md > Epic 2` criteria. Under 55 lines. Exactly three prong headings. Both worked failure cases present.
  Verify: `wc -l` under 55; `grep -c "^### Prong"` equals 3; confirm prong 2 states the skill-versus-nested distinction explicitly rather than listing them as alternatives.

- [ ] **3. The seven-section skeleton and the declined verdict**
  Spec ref: `spec.md > The skeleton`
  What to build: Create `references/skeleton.md` (~90 lines). The seven sections in order with status and a 2-3 line spec each. Gotchas carries the definition: a gotcha is an **unknown known**, obvious once named and invisible until named. One short real exemplar per section (2-4 lines) — **no `{placeholder}` fill-in-the-blank blocks**, which is the v0.2.1 pattern being replaced. Carry the persona-inheritance blockquote forward unchanged, all three cases. State the removals explicitly (What's where, Tech Stack, Common tasks, Design system, Voice) with the surviving-row rule. State the floor removal on "What NOT to do". Add the **"not yet worth a keystone"** verdict: first-class outcome, names what would earn one later, writes nothing.
  Acceptance: `prd.md > Epic 3` criteria. Under 100 lines. No skeleton placeholder tokens outside the persona blockquote. Declined verdict present and explicitly write-nothing.
  Verify: `wc -l` under 100; `grep -oE "\{[a-z0-9 -]+\}"` returns nothing except persona-blockquote name substitutions; `grep -i "not yet worth"` hits.

- [ ] **4. DOGFOOD GATE 1 — migrate `vibe-plugins/CLAUDE.md`**
  Spec ref: `spec.md > The gate` + `The guard` + `The skeleton`; `prd.md > Epic 6`
  What to build: Apply items 1-3 to a real file, cold. Read `c:/Users/estev/Projects/vibe-plugins/CLAUDE.md` (147 lines) in full. Classify every line: derivable-cut, gotcha, rationale, non-standard convention, or protected. Draft the candidate against the seven sections. Watch the known trap — the fifteen-row plugin roster is derivable and the file already carries the Node one-liner that regenerates it; the one-liner and the manifest-wins rule are keepers, the table is not. Show the diff. **Confirm with Este before writing.** Then apply, run the eight-item self-check, and record every hand-correction.
  Acceptance: Migrated file within the ~50/~100 budget. Gotchas non-empty. The tag-naming divergence, the `github` source-type SSH trap, the linear-promotion rule, and bot-owned `data/stats/` all survive. No protected content lost. Every hand-correction written to a running friction list.
  Verify: `wc -l` on the result; walk all eight self-check items out loud; confirm the four named gotchas are present by grep. **If the skeleton needed structural correction to produce a good file, stop and fix items 1-3 before item 5.**

- [ ] **5. Progressive disclosure**
  Spec ref: `spec.md > Progressive disclosure`
  What to build: Create `references/progressive-disclosure.md` (~75 lines). State the verified loading model (root and `.claude/CLAUDE.md` always; nested only when working under that directory; `.claude/rules/*.md` always), sourced to the v2.1.220 binary. Multi-surface detection signals: workspace manifests, multiple app roots, distinct deploy targets, multiple top-level toolchains. Nested keystones are proposed, never auto-written. The root-stays rule. Skill extraction as the escape valve. The budget: ~50 target, ~100 ceiling, **soft ceiling**, gotchas are never cut to hit it, a 40-line file of derivable content still fails. Fold in whatever item 4 taught.
  Acceptance: `prd.md > Epic 4` criteria. Under 85 lines. Loading model matches the verified quote. Budget section states the ceiling is soft and that gotchas are never cut for it.
  Verify: `wc -l` under 85; `grep -i "soft\|never cut"` confirms the budget guard; confirm the nested-versus-skill routing agrees with `protected-content.md` prong 2 rather than contradicting it.

- [ ] **6. DOGFOOD GATE 2 — migrate `Project-626Labs-1/CLAUDE.md`**
  Spec ref: `spec.md > Progressive disclosure`; `prd.md > Epic 6`
  What to build: The nested-keystone case, and the only real test of item 5. First **verify the actual surface boundaries** with `ls` and the repo's own manifests — do not assume the three surfaces named in the spec. Read the 302-line file in full, classify, draft a thin root plus per-surface nested proposals, apply the root-stays rule. Show the diff and the nested proposals. **Confirm with Este before writing.** Nested files are proposed; write them only on explicit approval, honoring the propose-never-auto-create rule this cycle is validating.
  Acceptance: `prd.md > Epic 4` and `Epic 6` criteria. Root within budget. Nested proposals map to real verified surfaces. Anything an agent needs on every task stayed at the root. Protected content intact.
  Verify: `wc -l` on the new root; confirm each proposed nested path exists as a directory; confirm no always-needed rule got buried in a subtree. **Record how well the detection signals and the root-stays rule actually held; that is the deliverable of this gate.**

- [ ] **7. Tenant interview, repo types, and capture**
  Spec ref: `spec.md > Architecture Overview`; `spec.md > Data Model`
  What to build: Three references. `tenant-interview.md` (~70) carries the v0.2.1 Step 1 interview forward and adds the second use of inherited docs — an **exclusion list**, not just a source, subject to guard prong 3; adaptation table updated to the new section names. `repo-types.md` (~45) reframes the four repo types around **what differs in gotcha shape**, not which sections to include, since section selection is now the gate's job and a type-to-section mapping would contradict it. `capture.md` (~50) moves Step 6 capture out of `SKILL.md` at `schema_version: 2` — new seven-name section vocabulary, `run_type` gains `declined`, three new fields (`nested_proposed`, `skills_proposed`, `root_line_count`), all four privacy rules carried verbatim, opt-in prompt preserved.
  Acceptance: `prd.md > Epic 3` and `Epic 5` criteria. Seven reference files now exist. `capture.md` shows `"schema_version": 2` and all four privacy rules.
  Verify: `ls references/` returns exactly seven files; `grep schema_version capture.md` shows 2; confirm `repo-types.md` contains no section-inclusion mapping.

- [ ] **8. Rewrite `SKILL.md`, update `evolve-keystone`, mark the harness proposals absorbed**
  Spec ref: `spec.md > Keystone's own SKILL`
  What to build: Rewrite `skills/keystone/SKILL.md` from 390 to ~100 lines (ceiling 130) per the eight-part structure in the spec. Frontmatter `description` rewritten and **unquoted** — escaped inner quotes blank the in-session listing, fixed in `4e8bea6`, must not regress. Trigger phrases preserved. Gate and guard in brief, each pointing at its reference. The eight-item self-check stays inline. "What you do not do" carried forward with the voice-rules item replaced by the guard and a new `/doctor` boundary item. Then update `evolve-keystone/SKILL.md`: skeleton now lives at `references/skeleton.md` not `SKILL.md` Step 2, and v1/v2 captures must not be pooled on section-name aggregation. Then update the `proposed-changes-harness.md` status header — #2 became this cycle's core, #3 and #4 shipped, #1 stays parked to hold the zero-scripts promise.
  Acceptance: `prd.md > Epic 5` criteria. `SKILL.md` under 130 lines. Every reference pointer resolves; every reference file is linked; no orphans. Frontmatter description unquoted.
  Verify: `wc -l SKILL.md`; loop every `references/*.md` pointer and confirm the file exists; loop every file in `references/` and confirm `SKILL.md` links it; `head -5 SKILL.md` shows an unquoted description value.

- [ ] **9. DOGFOOD GATE 3 — migrate the remaining three, consolidate friction**
  Spec ref: `prd.md > Epic 6`
  What to build: Migrate `Celestia3/CLAUDE.md` (205), `vibe-cartographer/CLAUDE.md` (204), and `Projects/CLAUDE.md` (90). **Before touching cartographer, check whether any section has a generator** (`grep -rn "CLAUDE.md" vibe-cartographer/scripts .github`) — the harness comparison flagged it as "half auto-generated," and cutting generated content without disabling the generator means it returns on the next run. For `Projects/`, treat the Marcus tenant wall and the duplicate-clone verification rule as protected: policy and hard-won operational knowledge, not derivable facts. Verify `Projects/` is even a git repo before attempting a commit. Diff and confirm each. Then write the consolidated friction list to `docs/v0.3-migration-friction.md`.
  Acceptance: `prd.md > Epic 6` criteria for all five files. Protected content verified surviving in each, per file, not assumed. Friction log names every hand-correction across all five migrations.
  Verify: `wc -l` each; confirm the tenant wall survives in `Projects/CLAUDE.md`; confirm no cartographer generator was orphaned. **This gate also tests Epic 5's untestable criterion: track whether these migrations forced repeatedly opening references that `SKILL.md` should have summarized. If so, the SKILL summary is too thin.**

- [ ] **10. Apply the skeleton fixes the dogfood demanded**
  Spec ref: `docs/v0.3-migration-friction.md`
  What to build: Read the consolidated friction log. Any defect that recurred across two or more migrations is structural — fix the reference tree. Any one-off is a note, not a change. This item exists as its own beat because "apply the lessons" is exactly the step that gets skipped when the build feels done, and the profile records that pattern.
  Acceptance: Every recurring defect either fixed or explicitly declined with a reason in the friction log. No silent drops.
  Verify: Re-read the friction log; each recurring item carries a disposition (fixed / declined-because). Re-run item 8's pointer-integrity checks if any reference changed.

- [ ] **11. Version, changelog, README, three descriptions, spec-bank back-merge**
  Spec ref: `spec.md > Runtime & Deployment`
  What to build: `plugin.json` to `0.3.0` with the description rewritten off the "626Labs-pattern / repo-type" framing. CHANGELOG entry leading with the breaking shape change, naming the `/doctor` evidence and the binary version, listing the seven sections, the removals, the guard, nested keystones, and the SKILL restructure, plus the five-repo validation with before and after line counts. README rewrite at lines 20-32 (the ten-section list), 38-43 (the repo-type table), and 53-69 (the "things you will NOT get" list, which references sections that no longer exist); update "Validated on" to name the five migrated repos. All three descriptions change together: `plugin.json`, the marketplace entry, and the SKILL frontmatter. Then back-merge into `vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` — it still states the superseded single-axis gate and lacks the four `/prd` corrections and the `/doctor` boundary resolution.
  Acceptance: `prd.md > Epic 7` criteria. `plugin.json` parses and reads 0.3.0. No stale section names survive in the README except where explicitly describing what was removed. Spec-bank doc states gate v2.
  Verify: `node -e` parse of `plugin.json`; `grep -n "What's where\|Common tasks\|Tech Stack" README.md` returns only intentional removal-describing hits; `grep -i "two-axis\|guess wrong" ../vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` hits.

- [ ] **12. Tag, verify, promote**
  Spec ref: `spec.md > Deployment — Identity & Signing`
  What to build: Push `main` on the solo repo. Tag `v0.3.0` — plain semver, **not** the `<plugin>-vX.Y.Z` form, which belongs only to `vibe-test` and `vibe-sec`. Push the tag. Verify with `gh api` that the tag resolves on the remote AND that `plugins/vibe-keystone/.claude-plugin/plugin.json` is fetchable at that ref. **Only then** bump `source.ref` from `v0.2.1` to `v0.3.0` in `vibe-plugins/.claude-plugin/marketplace.json` and rewrite that entry's description. Commit and push the promotion. Log the decision to the 626 dashboard: the shape change with its evidence basis, and the `/doctor` positioning call.
  Acceptance: `prd.md > Epic 7` criteria. Tag resolves. Manifest fetchable at the tag. Marketplace parses, `ref` reads `v0.3.0`, plugin count unchanged.
  Verify: `gh api repos/estevanhernandez-stack-ed/vibe-Keystone/git/refs/tags/v0.3.0 --jq .ref`; `gh api .../contents/plugins/vibe-keystone/.claude-plugin/plugin.json?ref=v0.3.0 --jq .name`; `node -e` on the marketplace confirming ref and plugin count. **If either gh check fails, stop — a ref pinned to a nonexistent tag breaks installs for every stable-channel user.**

- [ ] **13. Documentation & security verification**
  Spec ref: `prd.md > What We're Building` + `spec.md > all sections`
  What to build: **Adapted honestly to what this project is.** vibe-Keystone has no dependency manifest, no `.env`, no auth, no deployment secrets, and no executable code — verified this cycle: no `package.json`/`pyproject.toml`/lockfile, and zero `.js`/`.py`/`.sh` files. So `npm audit`, `.env.example`, input validation, auth checks, CORS, and HTTPS are all **N/A and marked as such rather than theatrically performed**. What genuinely applies: (a) README accurate against the shipped shape; (b) all `docs/` artifacts current, including this checklist reflecting what was actually built; (c) secrets scan across the working tree and git history; (d) `.gitignore` still correct; (e) **the real security surface for this plugin** — confirm the zero-scripts invariant still holds after the rewrite, confirm the only write surfaces are the target repo's `CLAUDE.md` and the opt-in `captures.jsonl`, confirm no network call was introduced anywhere, and confirm `PRIVACY.md` still describes the plugin truthfully at v0.3.0; (f) confirm the capture schema's privacy rules survived the move from `SKILL.md` to `capture.md` verbatim.
  Acceptance: README accurate. `docs/` current. No secrets in tree or history. Zero scripts confirmed post-rewrite. `PRIVACY.md` accurate at v0.3.0 or updated. Capture privacy rules intact and verbatim. Every N/A item recorded as N/A with its reason, not silently skipped.
  Verify: `find . -name "*.js" -o -name "*.py" -o -name "*.sh" | grep -v .git/` returns empty; `git log --all -p | grep -iE "password|secret|api_key|token"` shows nothing sensitive; `grep -ri "http://\|https://\|fetch\|curl" plugins/` returns only documentation links, no call sites; read `PRIVACY.md` against the v0.3.0 surface line by line.
