# CLAUDE.md — Phase 1: Reverse-Engineering (Brownfield → SDD)

Copy this file into the root of the target project repo as `CLAUDE.md` to begin Phase 1. It governs this phase only — once `/specs/baseline/ACCEPTED.md` exists, replace it with `ai-dlc-workflow/CLAUDE.md` to move into Phase 2.

---

## Rule 0 — Ask before assuming (applies to everything, always, and comes first)

If you are ever uncertain, missing context, or about to make an assumption — on ANY task, not just spec generation — **stop and ask me one clarifying question before deciding.** Do not proceed on a guess when asking is possible. This rule overrides convenience and overrides every other rule below if they ever conflict.

For larger or more ambiguous work (a new spec, a design decision, an unclear requirement, a confusing part of the existing code), use this fuller version instead of a single question:

```
Interview me to build a complete implementation spec. As you ask, identify
the core problem and who it's for, work through key decisions together,
and convert our conversation into a final spec. If I don't know an
answer, use your best judgment.
```

Never silently fill a gap with an assumption when you could ask.

---

## What this phase does

`/specs/baseline/` does not exist yet in this project. Do not start feature work.

Run `reverse-engineering-baseline.md` (from this same `playbook/` folder) step by step, in order, applying Rule 0 throughout. It produces `architecture.md`, `domain-model.md`, `business-rules.md`, `constitution.md`, `stories.md`, and `test-coverage.md` under `/specs/baseline/`, and ends with an explicit sign-off that creates `/specs/baseline/ACCEPTED.md`.

Optionally run `business-context-narrowing.md` (also in this folder) any time after Step 8 of that sequence, before the final sign-off, to sharpen the reverse-engineered stories with real business context.

---

## Next

Once `/specs/baseline/ACCEPTED.md` exists, Phase 1 is done. Copy `ai-dlc-workflow/CLAUDE.md` over this repo's root `CLAUDE.md` to move into Phase 2, which governs every enhancement and bug fix from here on.
