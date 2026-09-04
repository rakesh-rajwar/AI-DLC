# CLAUDE.md — Brownfield: Reverse-Engineering into SDD

Copy this file into the root of the target project repo as `CLAUDE.md` to begin. This is a **one-time process**: it governs the project only until the baseline is signed off. Once `/ai-dlc/ACCEPTED.md` exists, the project **becomes greenfield** — replace this file with `ai-dlc-workflow/greenfield/CLAUDE.md` and follow that workflow for every change from then on.

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

## What this does

`/ai-dlc/` does not exist yet in this project. Do not start feature work.

Run `reverse-engineering-baseline.md` (from this same `brownfield/` folder) step by step, in order, applying Rule 0 throughout. It produces the six baseline docs under `/ai-dlc/` (see that file's Step 10 for the canonical list) and ends with an explicit sign-off that creates `/ai-dlc/ACCEPTED.md`.

Optionally run `business-context-narrowing.md` (also in this folder) any time after Step 8 of that sequence, before the final sign-off, to sharpen the reverse-engineered stories with real business context.

---

## Next — this project becomes greenfield

Once `/ai-dlc/ACCEPTED.md` exists, the brownfield conversion is done — for good. This is a one-time process; there's no reason to come back to this file once the project has a baseline. Copy `ai-dlc-workflow/greenfield/CLAUDE.md` over this repo's root `CLAUDE.md`. From that point on, the project is indistinguishable from one that started greenfield — it follows the same standing workflow for every enhancement and bug fix.
