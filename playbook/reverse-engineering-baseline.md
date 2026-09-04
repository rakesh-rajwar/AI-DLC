# Reverse-Engineering a Legacy Codebase into a Spec Baseline

Run once per project, in order, before any spec-first feature work begins. Requires `CLAUDE.md` (from the repo root) to already be in place — Rule 0 in that file governs how Claude should handle ambiguity at every step below.

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
Based on the structural inventory, write /specs/baseline/architecture.md:
- overall architecture style (layered, MVC, microservices, etc.)
- data flow through the system for the 2-3 most important use cases
- tech stack and why each major dependency appears to be there
- integration points (databases, external APIs, queues, etc.)

Describe what exists. Do not suggest improvements or flag it as good/bad.
Apply Rule 0 if anything here is ambiguous.
```

## Step 3 — Domain model spec

```
Write /specs/baseline/domain-model.md: core entities, their attributes,
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
/specs/baseline/business-rules.md, with a file/line reference. Flag any
rule that looks like it might be a bug rather than intentional behavior
— don't decide which, just flag it.
```

## Step 5 — Constitution spec (conventions)

```
Write /specs/baseline/constitution.md: naming conventions, file/folder
structure patterns, error-handling style, testing patterns (if any), and
any conventions that are inconsistently followed. Note where the
codebase clearly disagrees with itself.
```

## Step 6 — Confidence check (verify before trusting)

```
For each doc in /specs/baseline/, list the 3-5 claims you're least
confident about, and why. Apply Rule 0 for any of these where my input
would resolve the uncertainty — don't just flag and move on.
```

Correct anything surfaced here before treating the baseline as ground truth.

## Step 7 — User stories (reverse-engineered from behavior)

```
Using /specs/baseline/domain-model.md and business-rules.md, write
/specs/baseline/stories.md: for each major user-facing capability you
can identify in the code, write it as a story in the form:

  "As a [role], I can [capability], so that [inferred benefit]"

followed by the acceptance criteria the code actually enforces today
(not what it should enforce — what it does). Group by feature area, and
cite the file/module each story is derived from.

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

---

## Next

Once Step 8 is clean, `/specs/baseline/` is ready to serve as ground truth. Optionally run `business-context-narrowing.md` next to sharpen and prioritize `stories.md` using real business context. Either way, all future work in this repo now follows the spec-first workflow defined in `CLAUDE.md`.
