# Builder Profile

<!-- Cart cycle #18. Returning builder — 17 completed cycles. Interview skipped per
     the returning-builder branch; fields sourced from ~/.claude/profiles/builder.json
     (last_updated 2026-05-22) plus this session's brainstorm. -->

## Who They Are

Estevan ("Mr. Solo Dolo"). Builder and outsider, runs 626Labs out of Fort Worth, TX. 20+ years on PC/Windows. Vibe coder: architects and ships through AI agents rather than writing code directly, with strong pattern recognition, troubleshooting, and systems thinking. Has shipped roughly ten deployed apps, three Microsoft Store titles, a portfolio site at 626labs.dev, and the Vibe plugin family this cycle targets.

Active Vibe Cartographer contributor. This is the 18th Cart cycle and the fifth aimed at a plugin in the family Cart itself builds.

## Technical Experience

**Level:** experienced.

**Languages:** TypeScript, Python, JavaScript, Luau, C#, HTML/CSS, C++.

**Frameworks:** React 19, Next.js, Vite, TailwindCSS, Firebase, FastAPI, Flask, Express, .NET 8/9, Azure, Expo, React Native, Drizzle ORM, Playwright, WPF, C++/WinRT, Windows App SDK / WinUI 3, MSIX, Ollama.

**AI agent experience:** deep. Builds and ships Claude Code plugins; runs multiple concurrent sessions; drives Workflow-orchestrated multi-agent builds.

**Relevant to this cycle:** authored the CLAUDE.md pattern being rewritten, across every repo in the estate. The files being migrated in this cycle are his own.

## Mode

**Builder.** Brisk pacing, minimal preamble, sharp-collaborator tone.

Persona: **architect** — strategic framing, tradeoffs surfaced, checkpoints at load-bearing forks only.

Autonomy: **fully-autonomous / self.** Flow through beats the profile and artifacts already answer; surface assumptions inline marked `(default — confirm on next interactive run)`. Opted in 2026-04-26 after 9 completed cycles.

## Project Goals

Adapt Vibe Keystone to the Claude 5 generation of models. Keystone generates the CLAUDE.md every agent decision in a repo rests on, and its skeleton was calibrated for an earlier model generation. The three sections it marks ALWAYS are the three Anthropic's own `/doctor` now names as cut targets; the three `/doctor` says to keep have no section in the skeleton at all.

The outcome: a Keystone that produces files cut against a derivability test, that knows about nested keystones and skill extraction, that protects human-supplied context from the trim, and whose own SKILL practices the progressive disclosure it teaches. Validated by migrating five real estate CLAUDE.md files.

Ships to the vibe-plugins marketplace as v0.3.0.

## Design Direction

No visual surface. This is a markdown-only plugin.

The applicable taste is prose discipline, and it is well established: builder-to-builder, second person, sentence case. Punchline first, support after. Specific over generic. No corporate speak. Em-dashes minimal; commas, periods, colons by default. No emoji in file content. No self-reference in body prose.

## Prior SDD Experience

Extensive. Seventeen completed Cart cycles plus independent spec-bank practice in vibe-plugins.

Deepening-round habit: zero rounds in 8 of 9 recorded cycles when the vision is formed. The recorded nuance matters here — one well-placed round at `/prd` on the Vibe Thesis cycle would have caught a false assumption before `/spec` authored a fork that needed mid-build undoing. Trade was 5 minutes against 30. This cycle's brief is unusually thorough, and the profile's own lesson (t) says thoroughness in a brief is a flag to verify upstream freshness *earlier*, not to skip verification.

## Architecture Docs

No separate architecture docs. The governing artifacts are:

- **Approved spec:** `vibe-plugins/docs/spec-bank/vibe-keystone-v0.3.md` (215 lines, design-approved 2026-07-25)
- **Implementation plan:** `vibe-plugins/docs/superpowers/plans/2026-07-25-vibe-keystone-v0.3.md` (12 tasks, 3 phases)
- **Family conventions:** `vibe-plugins/docs/conventions/` — operating doctrine, decision-log backend, model-tiering RFC
- **Target under rewrite:** `vibe-Keystone/plugins/vibe-keystone/skills/keystone/SKILL.md` (390 lines)
- **Privacy contract:** `vibe-Keystone/PRIVACY.md` — zero scripts, zero network. Load-bearing constraint on the whole cycle.

Evidence base for the rewrite is the shipped `/doctor` prompt read out of `claude.exe` v2.1.220, not the blog post describing it.
