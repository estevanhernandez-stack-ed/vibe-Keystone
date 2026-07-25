# Repo types

Type does not decide which sections appear. [`derivability-test.md`](derivability-test.md) decides that, line by line. What type tells you is **where to go looking for gotchas**, because different kinds of repos hide their traps in different places.

Use this to aim the inventory, not to fill a form.

## Code platform

Services, apps, libraries, plugins.

**Where the traps hide:** build and deploy ordering, things that must ship together, config a sibling file already establishes, generated or vendored directories, and any surface an agent could lock down without realizing something depends on it staying open.

**Ask:** what breaks if these two things deploy separately? Which directory looks editable and is not? What did someone already try that did not work?

## Marketing or content site

Public-facing, copy-heavy.

**Where the traps hide:** the publish pipeline, which files are authored versus rendered, asset paths that only resolve after a build step, and voice rules — which are protected content, not conventions.

**Ask:** what does the deploy actually run? Which of these files does a human edit? Where do the brand rules live, and is this repo their canonical home?

## Long-form writing or thesis

Prose-heavy, citation-bound.

**Where the traps hide:** citation integrity, mode or phase switching, the boundary between drafting and rendering, and commit vocabulary that diverges from software defaults.

**Ask:** what must never be fabricated? What does "done" mean for a chapter? Which directories are generated output?

The persona override is common here and is protected content. It is not a "section this type gets" — it is content the guard already covers.

## Infrastructure or mixed

Multiple surfaces under one root.

**Where the traps hide:** cross-surface coupling, the difference between a surface and a directory, duplicated or superseded copies of the same component, and rules that are true everywhere but written down in only one corner.

**Ask:** which of these directories are real surfaces and which are worktrees, archives, or scratch? What has to be coordinated across surfaces? Are any two of these the same thing?

This type is where the root-stays rule earns its keep. See [`progressive-disclosure.md`](progressive-disclosure.md).

## Any type

Two questions worth asking regardless:

- **What has already bitten someone here?** The best gotchas are scars.
- **What would a competent agent confidently get wrong on day one?** That is axis 2 of the gate, asked out loud.
