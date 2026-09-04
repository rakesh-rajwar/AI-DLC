# AI-DLC
Keep all info related to AI DLC

## Brownfield Projects.
Converting an undocumented, legacy brownfield software project into a highly disciplined, Spec-Driven Development (SDD) architecture using Claude Code requires a "planning-first" framework.
The code already exists, specs are missing or outdated, and adopting SDD feels almost impossible. That’s why we need Reverse-Spec.

## Reverse-Engineering a Legacy Codebase into AI-Assisted Specs — Reusable Playbook

A shared, project-agnostic process for bringing an existing (legacy) codebase into spec-driven, AI-assisted development using Claude Code — and keeping it that way as new work happens.

## Contents

```
CLAUDE.md                                  — drop into any project's root
playbook/
  reverse-engineering-baseline.md          — one-time: build /specs/baseline/
  business-context-narrowing.md            — optional: sharpen stories with business context
```

- **`CLAUDE.md`** — the standing rules Claude Code reads at the start of every session in a project: the forced ask-before-assuming rule (Rule 0), the baseline reference, the ongoing spec-first workflow, the lightweight bug-fix variant, and the learnings log.
- **`playbook/reverse-engineering-baseline.md`** — the ordered, copy-pasteable prompt sequence to run once against any existing codebase to produce `architecture.md`, `domain-model.md`, `business-rules.md`, `constitution.md`, `stories.md`, and `test-coverage.md` under `/specs/baseline/`, ending in an explicit sign-off (`ACCEPTED.md`) before spec-first work begins.
- **`playbook/business-context-narrowing.md`** — optional follow-up to prioritize and clarify the reverse-engineered stories using real product/business context. Skip if not available.

## How to apply this to a project

1. Copy `CLAUDE.md` into the root of the target repo (alongside this playbook, or referenced from wherever your team keeps shared docs).
2. Open Claude Code in that repo.
3. Work through `playbook/reverse-engineering-baseline.md`, step by step, in order — paste each step's prompt as-is.
4. (Optional) Run `playbook/business-context-narrowing.md` once the baseline is complete.
5. From here on, every enhancement or bug fix in that repo automatically follows the spec-first workflow defined in `CLAUDE.md` — including checking `/specs/learnings.md` first and updating the baseline after every change.

## Principle behind Rule 0

The single most important rule in `CLAUDE.md` is the first one: **ask before assuming, on anything, always.** Every other part of this process — the baseline being trustworthy, the specs staying in sync with code, the learnings log being accurate — depends on Claude asking instead of guessing whenever something is unclear. Teams adopting this playbook should not weaken or remove Rule 0.
