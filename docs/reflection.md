# Reflection — vibe-keystone v0.3.0, Cart cycle #18

**Shipped:** 2026-07-25, both channels. Marketplace ref `v0.2.1` → `v0.3.0`.
**Ran on:** Claude Opus 5, released the day before this cycle.

---

## Part A — the check-in

### Which artifacts did real work

The **checklist** was the one reached back to most, and specifically its stop conditions. Item 4's *"if the skeleton needed structural correction, stop and fix items 1-3 before item 5"* is what turned gate 2 from a blocked task into a caught defect rather than a workaround. The **PRD** was second, because several acceptance criteria were greppable and got run as literal commands.

`scope.md` read thinnest as a document, and that is misleading: its mandatory research beat is what caught the two-axis gate. The document was light; the act of producing it was not. Worth separating those when judging whether a phase earned its cost.

### Where Este changed the outcome

Four interventions, and three were the same move — **take a local finding and convert it to a class**:

| Saw | Said | Scope change |
|---|---|---|
| One stale em-dash line | *"that is probably the same in every repo"* | 1 file → 12 |
| A migration-time exception for the persona file | *"make sure that guard is on for all users"* | one case → a shipped rule |
| A 98-line persona | *"if the persona is weighed down by repeats it probably isn't worth it, especially if 5 is better suited already"* | one file → identity-vs-procedure |

The fourth, the shape-change notice, was the same instinct on a different axis: not *where else does this apply* but *who else is affected*. That is the one least likely to come from the agent side, because the agent is looking at artifacts and the builder is looking at users.

The em-dash case shows the cost of not making the move. The instance was found and stopped there; without the generalization, eleven repos would still hand agents a voice rule contradicting the global file.

### Cadence

Fourteen stops across the cycle. Este's read: correct **for this project**, because the terrain was genuinely novel — Opus 5 one day old, Fable 5 a few weeks, and the work was adapting a tool to that model generation.

This is the same principle already recorded for deepening rounds (*"context-dependent, not pattern-fixed: when the project's scope is genuinely fluid, deepening rounds earn their cost"*), generalized to check-in cadence. **Novelty of terrain sets the cadence, not a fixed preference.** Frontier work earns a tight loop; well-understood substrate does not.

---

## Part B — project review

*AI-generated observations. Use what is useful.*

### 1. Scope and idea clarity

**Landed.** The cuts are engineering cuts, not MVP trims. `scope.md` distinguishes them explicitly: *"the validator is cut because the invariant is more valuable than the feature"* — a tool promising "no scripts, no network, fully determined by SKILL files" that then ships a Node linter has broken something users chose it for. That is a scope decision made on architectural grounds and defended as such, which is rarer than trimming for time.

**Tighten.** The scope doc's own framing of the gate was single-axis, and it shipped that way into `prd.md` before the research beat corrected it. The correction arrived in the same command, so nothing propagated — but the sequence was *write the doc, then do the research*. Front-loading beat 2 would have caught it before the document existed rather than after.

### 2. Requirements thinking

**Landed.** The edge-case beat found four issues and one was a genuine design correction: skills and nested keystones treated as interchangeable destinations, when nested files are location-gated and skills are invocable anywhere. That would have shipped as a silent failure — marketing voice rules parked in `marketing/CLAUDE.md`, never loading when someone drafts copy from the repo root, no error. Zero formal deepening rounds, and the mandatory beat did the work a round would have.

**Tighten.** Epic 5's criterion *"an agent reading only SKILL.md behaves correctly"* was written with no mechanical check behind it, and `spec.md` flagged that honestly at the time. It shipped unverified. Writing an acceptance criterion that cannot be tested by the party writing it is a gap worth catching at PRD time, not at reflect time.

### 3. Technical decisions

**Landed.** The `/doctor` boundary was resolved by finding the right axis rather than defending the first framing. "Keystone births, `/doctor` maintains" did not survive the refresh case, and the replacement — **input**, not timing — held: Keystone reads the repo and can add; `/doctor` reads the file and can only subtract. The dogfood then turned it from an argument into evidence when classification surfaced two competing VS Code extensions that appear nowhere in the file.

**Tighten.** The stack-research beat verified the one assumption that could have sunk the architecture (`references/` subdirectories work — confirmed in two shipped plugins, one first-party). It did not verify the assumption that actually broke: that a tool-owned marker region can be located by substring. That cost a clobbered 101-line block. Assumptions about *how you will edit* deserve the same verification as assumptions about *what you will build*.

### 4. Plan versus reality

**Landed.** The plan's largest flaw was caught before execution. The inbound implementation plan batched all construction then all validation — integrate-at-end, the shape the builder profile records failing three times. `/checklist` restructured it into three interleaved dogfood gates. Every one of the cycle's critical findings came from a gate, and F-05 in particular was only findable because gate 2 ran against a real repo with real tooling before four more files had been built on the defect.

**Drift, and it was noticed.** Two items deviated from the checklist deliberately, both recorded rather than silently taken: gate 2 became a dry run instead of an applied migration (the target was generated), and item 13's verification ran *before* item 12's ship rather than after, because verifying a release after promoting it to the stable channel is backwards.

**Tighten.** Item 13's ordering problem was inherited from the template and only caught at execution. A checklist that ends with "documentation and security verification" after "tag and promote" is wrong for any project that ships mid-checklist. Worth fixing at `/checklist` time.

### 5. How the builder worked

**Landed.** Active throughout, and specifically active at the highest-leverage points. Every intervention changed the shape of what shipped rather than approving what existed. Three of four were generalizing moves; the fourth surfaced a user-facing concern the agent had not considered. No passive approvals of a diff.

**Tighten.** Nothing structural. One habit worth naming: the answers that moved the work most were the shortest ones. *"that is probably the same in every repo"* is eleven words and it changed the scope of a whole workstream. The instinct is already right; the only risk is second-guessing it into a longer answer.

---

## Goals check

`builder-profile.md` set the outcome as: a Keystone that produces files cut against a derivability test, that knows about nested keystones and skill extraction, that protects human-supplied context, and whose own SKILL practices the progressive disclosure it teaches — validated by migrating five real files.

All shipped. The honest accounting on the last one:

| | Before | After |
|---|---|---|
| `SKILL.md`, always loaded | 390 | 83 |
| `references/`, on demand | 0 | 494 across 8 files |
| Total content | 390 | 577 |

**The tree got bigger.** What shrank is what every invocation pays for, down 79 percent. That is the real result and it is smaller than "390 to 83" implies. Flagged at `/spec` as open issue 1 specifically so this document would report the honest number.

Five keystones resolved: `vibe-plugins` 147→46, `Celestia3` 205→140 (authored — it had no keystone at all), `vibe-cartographer` 204→152, `Projects` 90→52, `Project-626Labs-1` dry-run (generated file, correctly not applied).

---

## Cart versus superpowers, since this cycle ran both

Both toolsets ran over the same job by explicit choice. The comparison is a deliverable, so here it is honestly — with the caveat that this is **one run, two toolsets, same operator, non-identical halves.** An observation, not a measurement.

**The front halves overlap by design, not by accident.** Cart's guide anchors `superpowers:brainstorming` as the complement for `/scope`'s brain dump and `superpowers:writing-plans` for `/spec` and `/checklist` proposals. Running brainstorming and writing-plans before `/onboard` did not duplicate Cart's work; it did the work Cart would have delegated.

**What Cart added that superpowers did not have:**

- **A mandatory research beat.** `/scope` beat 2 caught the two-axis gate. superpowers' brainstorming has no equivalent — its "explore project context" reads as repo exploration, not external research. The beat that caught the defect is structural to Cart.
- **A builder profile with accumulated scar tissue.** Lessons (bbb) *dogfood earlier* and (ccc) *wire-as-you-build* are what indicted the inbound plan's ordering. Those lessons exist because seventeen prior cycles wrote them down. No amount of in-session reasoning produces that.
- **A retro that updates the profile.** The loop closes. superpowers' `finishing-a-development-branch` is about integration, not learning.

**What superpowers did better:** got to an approved design and a 12-task plan in two skills where Cart's front half is five commands and roughly 2,000 lines of process. For a well-understood job that is the better trade.

**The principle worth keeping:** the Claude 5 guidance deprecates **scaffolding**, not **instrumentation**. Keystone's ALWAYS sections were scaffolding — telling the model what it could already see — and they got cut. Cart's friction log, session log, and builder profile are sensors, and no amount of model capability makes last week's friction visible. Scaffolding gets cut as models improve. Sensors do not.

---

## Carried forward

**Unverified, not passed.** Epic 5's *"an agent reading only SKILL.md behaves correctly"* was never tested — the agent that ran the migrations wrote the references in the same session, so "did I reopen them" measures nothing. Needs a cold session given only `SKILL.md`, with reference-opens counted. Recorded in the dashboard decision's next-steps.

**Open, Este-owned.** The `626-tenets-thesis` pair is a diverged duplicate — same remote, two working copies, Publishing copy one commit newer. The em-dash fix landed in both so it is decoupled from the ruling, but which copy is home base is still unanswered.

**Deferred.** `.claude-personal/CLAUDE.md` (269 lines) was excluded from the migration set as a persona file rather than a repo keystone. Under the identity-versus-procedure rule it is now a more tractable target than it was at the start of the cycle.

**Next signal.** `captures.jsonl` needs 5+ v2 entries before `evolve-keystone` can propose skeleton changes from evidence rather than judgment.
