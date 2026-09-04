# Reverse-Engineering a Legacy Codebase into a Spec Baseline

Run once per project, in order, before any spec-first feature work begins. This is a one-time process — once it's done, the project follows the same standing workflow a greenfield project uses, for good. Requires `ai-dlc-workflow/brownfield/CLAUDE.md` to already be copied into the target repo's root as `CLAUDE.md` — Rule 0 in that file governs how Claude should handle ambiguity at every step below.

Project-agnostic — no edits needed unless you want to change file paths.

---

## Step 1 — Structural inventory (breadth before depth)

```
Scan this repository and produce a structural inventory: list all modules/
packages, their responsibilities (inferred from code, not comments), entry
points, external dependencies, and how the pieces connect. Don't go deep
into any one module yet — I want the map first.
```

## Step 2 — Architecture spec

```
Based on the structural inventory, write /.ai-dlc/baseline/architecture.md:
- overall architecture style (layered, MVC, microservices, etc.)
- data flow through the system for the 2-3 most important use cases
- tech stack and why each major dependency appears to be there
- integration points (databases, external APIs, queues, etc.)
- build/test/deploy pipeline and environments, if any exist (CI config,
  Dockerfiles, deploy scripts) — describe what runs, when, and where

Describe what exists. Do not suggest improvements or flag it as good/bad.
Apply Rule 0 if anything here is ambiguous.
```

## Step 3 — Domain model spec

```
Write /.ai-dlc/baseline/domain-model.md: core entities, their attributes,
and relationships, as actually implemented (check the data layer, not
just naming conventions in code — those can be misleading in legacy
code). Note any entities that appear to serve overlapping or unclear
purposes.
```

## Step 4 — Business rules spec (the part manual docs usually miss)

```
Search the codebase for business logic that isn't obvious from naming:
validation rules, edge-case handling, special-cased conditionals, and
calculations. For each one, write it as a plain-language rule in
/.ai-dlc/baseline/business-rules.md, with a file/line reference. Flag any
rule that looks like it might be a bug rather than intentional behavior
— don't decide which, just flag it.
```

## Step 5 — Constitution spec (conventions)

```
Write /.ai-dlc/baseline/constitution.md: naming conventions, file/folder
structure patterns, error-handling style, testing patterns (if any), and
any conventions that are inconsistently followed. Note where the
codebase clearly disagrees with itself.
```

## Step 6 — Confidence check (verify before trusting)

```
For each doc in /.ai-dlc/baseline/, list the 3-5 claims you're least
confident about, and why. Apply Rule 0 for any of these where my input
would resolve the uncertainty — don't just flag and move on.
```

Correct anything surfaced here before treating the baseline as ground truth.

## Step 7 — User stories (reverse-engineered from behavior)

```
Using /.ai-dlc/baseline/domain-model.md and business-rules.md, write
/.ai-dlc/baseline/stories.md: for each major user-facing capability you
can identify in the code, write it as a story entry in this format:

  ### <Story title>

  As a [role], I can [capability], so that [inferred benefit].

  **Status:** Done
  **Source:** <file/module this was derived from>

  **Acceptance criteria:**
  - <criterion the code actually enforces today — not what it should
    enforce, what it does>

Every entry is "Status: Done" — it already exists and works, by
definition, since it's derived from running code — so there's no
technical-details or task section to fill in (those only apply to
stories drafted for future work, per greenfield/CLAUDE.md). "Source"
is the file/module citation, standing in for the intake-file link a
newly planned story would have. Group entries by feature area.

Where the code implies a capability but the "so that" purpose is
unclear, mark it [PURPOSE UNCLEAR] rather than guessing — or apply Rule
0 if resolving it needs my input.
```

## Step 8 — Cross-check stories against business rules

```
Cross-check stories.md against business-rules.md — flag any business
rule that isn't reflected in any story (dead/orphaned logic) and any
story whose acceptance criteria aren't backed by a rule you found
(possible undocumented assumption).
```

This is usually where legacy codebases surface their ugliest surprises — orphaned logic nobody uses, or behavior nobody wrote down as intentional.

## Step 9 — Test coverage baseline (the safety net greenfield gets for free)

```
Inventory the existing automated tests: what's covered, by module/area,
and what has no coverage at all. Write /.ai-dlc/baseline/test-coverage.md:
coverage by module, test types present (unit/integration/e2e), and a
list of high-risk untested areas — cross-reference against
business-rules.md so rules with no test backing them are called out
specifically. Don't write missing tests yet — just map the gap.
```

A greenfield project starts with no legacy behavior to break, so there's nothing to protect yet. A brownfield one already has real behavior in production — `test-coverage.md` is what lets future spec-first changes touch untested code safely instead of by accident. `ai-dlc-workflow/greenfield/CLAUDE.md`'s spec-first workflow references this file when deciding whether a task needs a characterization test before the real change.

## Step 10 — Baseline sign-off (the gate before spec-first mode)

This is the canonical list of the six baseline docs — `README.md` and the other files in
`ai-dlc-workflow/` refer back to it rather than repeating it. If you add, rename, or remove a
baseline doc, update it here first, then check `greenfield/CLAUDE.md`'s "Baseline reference"
section (its own copy, kept for standalone use without this file present) for the same change.

```
Baseline is complete: architecture.md, domain-model.md, business-rules.md,
constitution.md, stories.md, and test-coverage.md all exist and Steps 6
and 8 are clean. I'm going to review all of them now. Once I confirm,
create /.ai-dlc/baseline/ACCEPTED.md containing just the date and the line
"Baseline accepted as ground truth on <date>." Do not treat the baseline
as ground truth, and do not begin spec-first feature work, until I've
given that confirmation and this file exists.
```

Don't skip this — it's the one explicit moment a human, not just a clean cross-check, says "yes, this is right." Everything after this point in `ai-dlc-workflow/greenfield/CLAUDE.md`'s spec-first workflow assumes the baseline is trustworthy; this step is where that trust is actually earned rather than assumed.

---

## Next — this project becomes greenfield

Once Step 10's `ACCEPTED.md` exists, `/.ai-dlc/baseline/` is ground truth and the brownfield conversion is complete — for good; there's no reason to run this sequence again. Copy `ai-dlc-workflow/greenfield/CLAUDE.md` over this repo's root `CLAUDE.md` (replacing this brownfield file). From here on the project follows the same standing spec-first workflow a greenfield project uses. Optionally run `business-context-narrowing.md` any time after Step 8 — and before Step 10's sign-off, since it revises `stories.md` — to sharpen and prioritize stories using real business context.
