# Process Notes — vibe-keystone v0.3 (Cart cycle #18)

Project: the derivability rewrite. Target repo `vibe-Keystone`, ships to the vibe-plugins marketplace as v0.3.0.

## /onboard

**Returning builder.** 17 completed cycles on file. Full 11-step interview skipped per the returning-builder branch. Profile sourced from `~/.claude/profiles/builder.json`.

**Config in force this cycle:**

- persona `architect`, mode `builder`, tone terse and direct, pacing brisk
- `autonomy_level: fully-autonomous`, `cycle_builder_identity: self` — Autonomous-Self. Flow through beats the record answers; surface assumptions inline.
- `deployment_target: vibe-plugins-marketplace` — already correct, no re-ask needed
- `build_mode_preference: iterative-prototype`

**Decay deferred.** Profile `last_updated` is 2026-05-22, 64 days before this run, and the profile carries no `shared._meta` block, so the 1.4.x-to-1.5.0 fresh-stamp migration is outstanding. Per the fully-autonomous contract rule 4 (defer stale decay-stamps, never block an autonomous run on housekeeping), both the stamp migration and any TTL re-validation are deferred to the next guided run. Additional reason to defer rather than run silently: the profile is ~114KB and a mid-run rewrite carries more downside than the stale stamps do. `(default — confirm on next interactive run)`

**Session logger not wired.** This is an orchestrator-context run: one chat driving the whole command chain from outside Cart's own runtime. `session-logger.start()` has no runtime here, so no session-log entries are being written. `process-notes.md` is the durable record. `/vibe-cartographer:reconnect` at the end of the cycle backfills the log from these notes.

**Entering with artifacts already built.** This cycle did not start from a blank page. Before Cart was invoked, the session ran `superpowers:brainstorming` to an approved design and `superpowers:writing-plans` to a 12-task plan:

- `vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` — 215 lines, design-approved
- `vibe-plugins/docs/superpowers/plans/2026-07-25-vibe-keystone-v0.3.md` — 12 tasks, 3 phases

Este chose the full Cart arc over entering mid-arc, explicitly to compare the two processes under Claude 5 models. That comparison is a deliverable of this cycle, not a side effect.

**Composition note, surfaced at onboard because it reframes the comparison.** Cart's guide already anchors `superpowers:brainstorming` as the complement for `/scope`'s brain dump and `superpowers:writing-plans` as the complement for `/spec` and `/checklist` proposal phases. Those are the two skills that already ran. So the front half of the arc is not duplicated effort: Cart would have delegated those phases to exactly these complements. The arc's remaining job is to sharpen their output into Cart's artifact contract and add what superpowers has no equivalent for — friction capture, session memory, and the `/reflect` retro.

**Design decision carried in from the brainstorm** (recorded here because downstream commands depend on it):

1. Scope is output + Keystone's own SKILL + a 5-repo estate migration.
2. The new shape replaces the old skeleton. No lean/full mode toggle. v0.3.0.
3. Audience is frontier-model-first; tier-portable procedure moves to referenced files rather than staying always-loaded.
4. `/doctor` complements rather than competes — verified from the shipped binary, not assumed from the blog.
5. The protected-content guard ships for all users, not as a migration exception. Three prongs: the derivability test has no authority over human-supplied context; protected content routes by frequency of need, never deleted for length; dedup points at the canonical copy and never deletes the last one.

**Flaw found in the inbound plan, at onboard.** The profile's accumulated lessons indict the plan's phase ordering. Recorded lessons (bbb) "dogfood earlier", (ccc) "wire-as-you-build, not integrate-at-end", and the three-time-recurring "structural-green != works" all describe the same failure. The plan builds seven reference files and a SKILL rewrite in Phase 1, then migrates five real files in Phase 2 — integrate-at-end, the exact shape that has burned three prior cycles. Fix belongs at `/checklist`: the first real migration moves up to immediately after the skeleton reference lands, so the skeleton is proven against a real file while it is still cheap to change.

**Open, owned:**

- Decay stamp migration — deferred to next guided run. Owner: next interactive session.
- Session-log backfill — `/reconnect` at cycle end. Owner: this cycle.
- `.claude-personal/CLAUDE.md` (269 lines) is excluded from the migration set. It is a persona file, almost entirely protected content, and needs a separate judgment pass with Este. Owner: Este, post-cycle.

## /scope

**Beats flowed through, one researched.** Under the fully-autonomous contract, the brain dump, the gap-sharpening, and the what's-cut beats were all answered by the approved spec and the builder profile, so they were not re-asked. Beat 2 (research and reaction) was genuinely unanswered and was run.

**The research beat changed the design.** Este linked two Anthropic articles at kickoff but pasted only one in full. The unread one — the Fable field guide, "finding your unknowns" — carried a passage the spec had no account of:

> "If you are too specific, Claude will follow your instructions even when a pivot may be more appropriate. If you are too vague, Claude will often make choices and assumptions based on industry best practices that may not be a fit for your task."

The inbound spec was one-sided. Everything in it applied cut pressure, and over-trim was handled by a weak mitigation ("Orientation and Pointers stay ALWAYS") with no mechanism behind it. The field guide names the mechanism: when context is thin, the model silently substitutes industry defaults. That is *what* breaks on over-trim, and it converts the gate from one axis to two.

**Gate v2 (supersedes the spec's single-axis version):**

> Cut what Claude can derive. Keep what Claude would otherwise guess wrong.

Cut requires BOTH *derivable* AND *the default would be right*. A line whose absence would produce a wrong default assumption stays, even when technically derivable.

**Concrete defect this fixes.** "The load-bearing artifact is `.claude-plugin/marketplace.json`" is derivable in principle — read every file and infer it. Single-axis says cut. But the industry-default assumption is that the README or `package.json` is the center of a repo, and here that is wrong. Single-axis was going to trim load-bearing content out of every keystone the tool shipped. Caught before `/prd`, not mid-build.

**Second gain from the same source.** The field guide's four-quadrant frame supplies a definition for the Gotchas section that the spec was missing: a gotcha is an **unknown known** — an "obvious-until-you-see-it detail." Obvious the moment it is named, invisible until then. That is a sharper test for what belongs in the section than "traps and invariants."

**Process observation for `/reflect`.** This is the clearest evidence so far on the Cart-versus-superpowers comparison. `superpowers:brainstorming` produced a design Este approved, and it was wrong in one load-bearing way. Cart's `/scope` has a mandatory research beat; superpowers' brainstorming has no equivalent, and its checklist item 1 ("explore project context") reads as repo exploration, not external research. The beat that caught this is structural to Cart, not luck. Cost: one WebFetch.

**Deepening rounds:** zero, consistent with 8-of-9 recorded habit. Justified here because the research beat already did the work a deepening round would have done — it surfaced the unknown unknown rather than polishing the known.

**Active shaping:** Este drove two decisions this cycle that the record now depends on. He generalized the protected-content guard from a migration exception to a shipped rule for all users ("we need to make sure that guard is on for all users"), and he chose the full Cart arc over entering mid-arc specifically to get the process comparison. Both were course corrections on my proposals, not ratifications.

**Open:** none new. Gate v2 propagates into `/prd` and `/spec`; the spec-bank document at `vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` states the single-axis version and needs a back-merge before ship.

## /prd

**Beats flowed through except edge cases, which were run properly.** Scope walk-through, user stories, and acceptance criteria were all derivable from `scope.md` and the approved spec. Beat 4 (edge cases) and beat 5 (scope guard) were genuinely unanswered, and the profile's Vibe Thesis lesson is specifically that one well-placed round at `/prd` would have saved 30 minutes of mid-build re-architecting. Ran it.

**Four finds, one of them a design correction.**

1. **Nested keystones and skills are not interchangeable destinations.** This is the correction. Prong 2 of the guard says situational protected content "moves to a skill or a nested keystone under the subtree it governs" — treating the two as equivalent. They are not. Nested files are **location-gated**: they load only when work happens under that directory. Skills are **invocable from anywhere**. Park marketing voice rules in a nested file under a marketing subtree and they fail silently the first time someone writes marketing copy from the repo root. Corrected: protected-but-situational content defaults to a **skill**; a nested keystone only when the work genuinely never leaves that subtree. Would have shipped as a silent failure mode.

2. **The empty-repo case had no defined behavior.** Gotchas is ALWAYS, but a greenfield repo has none. The old skeleton would emit headings with nothing under them. Added a first-class **"not yet worth a keystone"** verdict, directly modeled on Vibe-Walk's "don't build a tour" verdict — which the profile records as that cycle's differentiator. Same shape, same reasoning: a tool that can decline to fire is more trustworthy than one that always produces output.

3. **The budget could eat real content.** A ~100-line ceiling against a repo with twenty genuine root-level gotchas. Nothing in the spec said which wins. Resolved: the ceiling is soft, gotchas are never cut to hit budget, and the budget squeezes derivable content only. Stated as a rule rather than left to judgment, because "hit the budget" is the kind of instruction an agent follows past the point of sense.

4. **The two axes are not equally checkable.** Axis 1 (derivability) is mechanical. Axis 2 (would the default be wrong) is inferential. Blurring them would let a judgment call masquerade as a mechanical check. The split is now stated, with the tie-break: uncertain on axis 2 means keep.

**Scope guard.** Nothing grew. The one thing that tried to was the drift-doctor, which the empty-repo verdict discussion pulled toward — a verdict about whether a repo *still* deserves its keystone is one step from rot detection. Held the line: the verdict fires at write time, drift detection stays deferred as a separate product.

**Open question raised to `/spec`, load-bearing.** "Keystone births, `/doctor` maintains" is too clean. The five migrations in Epic 6 *are* maintenance on existing files. Proposed resolution: Keystone **regenerates** (re-derives from the repo, diffs, confirms) and `/doctor` **trims** (edits in place). Different operations on the same artifact. `/spec` has to state it or the positioning falls apart the first time a user runs `/keystone` on a repo that already has one.

**Deepening rounds:** zero formal rounds; the mandatory edge-case beat did the work. Consistent with the recorded habit and with the reframe Este endorsed 2026-04-24 — the value of the round is the asking, and the asking happened.

**Active shaping:** none this beat. Este has not been in the loop since approving the arc; every decision above is agent-side and marked accordingly. All four finds are surfaced for his async review per the Autonomous-Self contract, and the `/doctor` boundary question is the one I would most want him to look at.

## /spec

**Stack research beat run on the one thing that could have sunk the architecture.** The entire progressive-disclosure restructure rests on a skill being able to load files from a `references/` subdirectory. If that pattern were not supported, the SKILL rewrite has no destination and Epic 5 collapses. Verified rather than assumed: the pattern ships in `vibe-cartographer/plugins/vibe-cartographer/skills/guide/references/` (live in the 626 marketplace) and in Anthropic's own `superpowers/6.2.0/skills/using-superpowers/references/`. Two independent precedents, one of them first-party. Bet is sound.

**Open question 1 resolved — the `/doctor` boundary.** The PRD flagged this as needing an answer before `/spec`, and the slogan did not survive: "Keystone births, `/doctor` maintains" fails the moment someone runs `/keystone` on a repo that already has a CLAUDE.md, which is exactly what the five migrations in this cycle are.

The distinction that holds is the **input**, not the timing. Keystone's input is the repo; `/doctor`'s input is the file. Keystone **regenerates** — re-derives a candidate from the repo, diffs, confirms, writes — and can therefore surface a gotcha nobody ever wrote down. `/doctor` **trims** — proposes deletions of derivable content and edits in place — and can only remove or relocate what is already present. That is a real capability difference, and it means both tools have a legitimate job on the same fat file without either being redundant.

Deliberately not built: detection and redirection between the two. A user can navigate this themselves; the skill states the distinction and points at `/doctor` for in-place trimming. Over-engineering a boundary that does not need policing.

**Open question 4 resolved — the declined verdict writes nothing.** Report only. Writing a stub file would recreate the empty-skeleton problem the verdict exists to prevent.

**Architecture self-review surfaced five open issues, and one is a correction to my own claim.** Issue 1: the restructure does not make the skill free. `SKILL.md` at ~100 lines is still always-loaded once the skill fires; what moved is the depth, from every-run to runs-that-need-it. That is the actual win and it is smaller than "390 lines to 100" implies. Flagged so `/reflect` reports the real number rather than the flattering one.

Issue 3 is the one with teeth: **there is no mechanical test for "an agent reading only SKILL.md behaves correctly,"** which is an Epic 5 acceptance criterion. The honest verification is behavioral — if the estate migrations require repeatedly opening references that `SKILL.md` should have summarized, the summary is too thin. That gets measured from the friction log at `/reflect`, not asserted at build time.

**Self-review ran inline, not via subagent.** The spec SKILL annotates architecture self-review as `judgment` tier and suggests subagents. Este's standing instruction in this session is no Agent tool unless he asks. Honored; the review ran inline. Noting it because the annotation exists and the deviation was deliberate.

**Deepening rounds:** zero. The mandatory beats plus self-review covered the ground, and the two genuinely open questions from `/prd` both got resolved inside the mandatory flow rather than needing an extra round.

**Deployment section:** captured the GitHub-release row plus the marketplace pin. Recorded the tag-scheme trap explicitly — `vX.Y.Z` plain semver here, NOT the `<plugin>-vX.Y.Z` form, which belongs only to `vibe-test` and `vibe-sec` from their `git-filter-repo` extraction lineage. Existing tags confirm the plain form. That is exactly the class of non-derivable convention this whole cycle is about, which is a small piece of evidence that the gate's keep-criteria are pointed at real things.

## /checklist

**The sequencing flaw is fixed, and fixing it was the whole job of this command.** The inbound plan built seven references plus the SKILL rewrite, then migrated five real files. Integrate-at-end. The builder profile records that exact shape failing three times: (bbb) dogfood earlier, (ccc) wire-as-you-build, and "structural-green != works" — logged as "now a law" after Vibe-Lingual, where a full end-to-end dogfood caught three P0s that 245 passing unit tests missed.

The checklist interleaves instead. **Three dogfood gates at items 4, 6, and 9.** Item 4 runs the first real migration immediately after the gate, guard, and skeleton exist, before anything else is written — so a structural defect surfaces on a 147-line file whose gotchas are already well characterized, not on the 302-line one after four more files have been built on the bad foundation. Item 6 is the only real test of nested keystones. Item 9 validates the fully assembled thing.

Each gate carries an explicit stop condition. Item 4's is the sharpest: *if the skeleton needed structural correction to produce a good file, stop and fix items 1-3 before item 5.*

**Item 10 exists because "apply the lessons" is the step that gets skipped.** The friction log is worthless if nothing reads it. Making the application of recurring fixes its own numbered item with its own acceptance criteria is the cheapest available guard against the build feeling done before it is.

**13 items, over the 8-12 guideline.** The overage is exactly the three gates. Deliberate, recorded here rather than silently exceeded.

**Verification overrides autonomy on one axis.** Autonomous mode plus `fully-autonomous` would normally mean flowing through without confirmations. Five of these items overwrite CLAUDE.md files in live repos Este owns. Keystone's own rule is never to overwrite an existing CLAUDE.md without a diff and a confirm, and a cycle validating that rule cannot be the thing that breaks it. Every migration diff gets confirmed. The autonomy contract's own language supports this: confirmations exist for genuine forks the record cannot resolve, and "is this rewrite of your live keystone correct" is not something the record can answer.

**Item 13 adapted rather than performed.** The template's security item assumes an app: `npm audit`, `.env.example`, input validation, auth, CORS, HTTPS. Verified this cycle that vibe-Keystone has no dependency manifest and zero `.js`/`.py`/`.sh` files, so those checks are **N/A and recorded as N/A with reasons**, not theatrically run. The genuine security surface for this plugin is different and now explicit: does the zero-scripts invariant still hold after the rewrite, are the only write surfaces still the target `CLAUDE.md` plus the opt-in `captures.jsonl`, was any network call introduced, does `PRIVACY.md` still describe the plugin truthfully at v0.3.0, and did the capture privacy rules survive the move to `capture.md` verbatim. That is a real audit for a markdown plugin; running `npm audit` on a repo with no manifest would have been a checkbox.

**Deepening rounds:** zero, matching the recorded builder-mode pattern of consistently skipping rounds at `/checklist` specifically. The sequencing question that would have justified a round was already answered by the profile's own lessons.

**Active shaping:** none. Este invoked `/checklist` and let it run. All decisions agent-side, surfaced here.

**Open:** the spec-bank document at `vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` still states the superseded single-axis gate and is missing the four `/prd` corrections plus the `/doctor` boundary resolution. Back-merge is item 11, not deferred past ship.
