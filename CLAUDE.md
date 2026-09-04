# CLAUDE.md — Project Rules (AI-Assisted Spec-Driven Development)

Copy this file into the root of any project repo. These rules apply to every session, automatically — this is what Claude Code reads before doing anything else.

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

## Baseline reference

`/specs/baseline/` is ground truth for this project once it exists:
- `architecture.md`
- `domain-model.md`
- `business-rules.md`
- `constitution.md`
- `stories.md`

**If `/specs/baseline/` does not exist yet**, do not start feature work. Point me to `playbook/reverse-engineering-baseline.md` in this repo and run that sequence first, step by step, applying Rule 0 throughout.

---

## Spec-first workflow (post-baseline)

Before starting ANY new work — enhancement or bug fix:

0. Check `/specs/learnings.md` first. If a past entry is relevant to this area of the code, say so and factor it in before proceeding. If the file doesn't exist yet, skip this check — it gets created the first time step 4 below runs.

1. Check `/specs/baseline/stories.md` and `business-rules.md` — confirm whether this request is genuinely new, or already exists / is a variation of existing behavior. State which, before proceeding.

2. For anything genuinely new or changed (not a lightweight bug fix — see variant below), create `/specs/<change-name>/` and go through, in order:
   - `requirements.md` — what's being asked, acceptance criteria
   - `design.md` — how it fits with the existing architecture and domain model in `/specs/baseline/`
   - `tasks.md` — ordered, small implementation tasks, each tagged to a requirement
   - implementation — one task (or small task group) at a time; show the diff and wait for approval before continuing

3. After implementation, update `/specs/baseline/` to reflect the change:
   - add/update the relevant story in `stories.md`
   - add any new business rule to `business-rules.md`
   - update `architecture.md` or `domain-model.md` if the change affects structure or entities

4. Append an entry to `/specs/learnings.md` (see below).

Never skip straight to code. Never treat `/specs/baseline/` as static — it must stay in sync with what the code actually does after every change.

---

## Bug-fix variant (lightweight path)

For bug fixes that do NOT change architecture, domain model, or existing contracts, skip `design.md` — use this shorter sequence instead:

1. `requirements.md` (lightweight) — what's broken, expected vs actual behavior, and the acceptance criterion for "fixed." Reference the business rule or story this violates, if any.
2. `tasks.md` — usually a single task: root cause + fix + regression test.
3. implementation — fix + test, show diff, wait for approval.

If, while investigating, the fix turns out to require an architecture or domain-model change (i.e. it's bigger than it looked), **stop and say so** — switch to the full `requirements → design → tasks → implementation` flow instead of proceeding on the lightweight path.

---

## Learnings log

After completing any bug fix or enhancement, append an entry to `/specs/learnings.md`:

- Date, what changed (link to `/specs/<change-name>/` or the bug fix)
- Root cause (for bug fixes) or key design decision (for enhancements)
- Any baseline doc that was corrected or updated as a result
- Anything surprising or non-obvious that future work on this area should know

Keep entries short — a few lines each. This file is append-only; don't edit past entries. Always check this file (step 0 above) before starting new work — the answer to "why does this module do X" often lives here rather than in the code itself.
