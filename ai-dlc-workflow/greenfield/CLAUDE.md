# CLAUDE.md — Greenfield: AI DLC Workflow (Spec-Driven Development)

Copy this file into the root of the project repo as `CLAUDE.md`. For a project that started greenfield, do this on day one. For a project that started brownfield, do this once `/specs/baseline/ACCEPTED.md` exists, replacing `ai-dlc-workflow/brownfield/CLAUDE.md` — from that point the project is greenfield too, and this file is all it ever needs going forward. These rules apply to every session, automatically — this is what Claude Code reads before doing anything else.

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
- `test-coverage.md` — coverage map used to decide when a change needs a characterization test first (see spec-first workflow below)
- `ACCEPTED.md` — presence of this file is the signal the baseline was reviewed and accepted, not just generated

**If `/specs/baseline/` does not exist yet, or exists but has no `ACCEPTED.md`**, do not start feature work — figure out which case this is, asking me if it's not obvious:
- **Existing code with no specs (brownfield)** — stop and tell me. Go get `ai-dlc-workflow/brownfield/CLAUDE.md` and run its `reverse-engineering-baseline.md` sequence first, ending with the sign-off that creates `ACCEPTED.md`.
- **A genuinely new project (greenfield from day one)** — there's no code to reverse-engineer. Use Rule 0's interview approach to write `/specs/baseline/` directly with me, then create `ACCEPTED.md` once I confirm it, before starting feature work.

---

## Spec-first workflow (post-baseline)

Before starting ANY new work — enhancement or bug fix:

0. Check `/specs/learnings.md` first. If a past entry is relevant to this area of the code, say so and factor it in before proceeding. If the file doesn't exist yet, skip this check — it gets created the first time step 4 below runs.

1. Check `/specs/baseline/stories.md` and `business-rules.md` — confirm whether this request is genuinely new, or already exists / is a variation of existing behavior. State which, before proceeding.

2. For anything genuinely new or changed (not a lightweight bug fix — see variant below), create `/specs/<change-name>/` and go through, in order:
   - `requirements.md` — what's being asked, acceptance criteria
   - `design.md` — how it fits with the existing architecture and domain model in `/specs/baseline/`
   - `tasks.md` — ordered, small implementation tasks, each tagged to a requirement. If `/specs/baseline/test-coverage.md` marks the affected area as untested, add a task to write a characterization test capturing current behavior before the task that changes it.
   - **plan** — enter Plan Mode and lay out the implementation plan built from `tasks.md`, checked against `/specs/baseline/` (architecture, domain model, business rules, conventions). As part of this plan, draft the full new/updated `stories.md` entry in the same format as the rest of the file — "As a [role], I can [capability], so that [benefit]" plus the complete acceptance criteria this change commits to (drawn from `requirements.md`), not a stub. This is what the plan commits to delivering, not a write-up invented after the fact. Once the story is written, stop and explicitly ask me to review and confirm it — separately from the rest of the plan if useful. Do not write any code until I've confirmed the story and approved the plan.
   - implementation — one task (or small task group) at a time; show the diff and wait for approval before continuing

3. Once implementation is done, verify it before calling it finished — run the full test suite, linters/build, and manually check the plan's acceptance criteria against what was actually built. Don't rely on a single passing test as proof; use every check the repo has.

4. Update `/specs/baseline/` to reflect the change, in the same change — never as a follow-up:
   - confirm the story and acceptance criteria drafted during planning still match what was actually built; correct them if implementation diverged from the approved plan, then commit that full entry to `stories.md`
   - add any new business rule to `business-rules.md`
   - update `architecture.md` or `domain-model.md` if the change affects structure or entities
   - update `test-coverage.md` if the change added or changed tests

5. Append an entry to `/specs/learnings.md` (see below).

**No code change is complete, and none gets committed or pushed, until steps 3-5 above are done.** Verification, baseline docs, and the learnings entry ship together with the code — not after it, not "in a follow-up." Never skip straight to code. Never treat `/specs/baseline/` as static — it must stay in sync with what the code actually does after every change.

---

## Bug-fix variant (lightweight path)

For bug fixes that do NOT change architecture, domain model, or existing contracts, skip `design.md` — use this shorter sequence instead:

1. `requirements.md` (lightweight) — what's broken, expected vs actual behavior, and the acceptance criterion for "fixed." Reference the business rule or story this violates, if any.
2. `tasks.md` — usually a single task: root cause + fix + regression test. If `/specs/baseline/test-coverage.md` marks the affected code as untested, first write a characterization test capturing today's (buggy) behavior, then the regression test proving the fix — same task.
3. **plan** — enter Plan Mode with the fix plan built from `tasks.md`, checked against `/specs/baseline/`. Wait for explicit approval before writing any code.
4. implementation — fix + test, show diff, wait for approval.
5. Verify — run the full test suite (not just the new regression test) and linters/build before calling the fix done.
6. Update `/specs/baseline/business-rules.md` or `stories.md` if the fix corrects a documented rule/behavior, and `test-coverage.md` if coverage changed — same change, not a follow-up. Append the learnings entry (below).

**No fix is complete, and none gets committed or pushed, until steps 5-6 above are done.**

If, while investigating, the fix turns out to require an architecture or domain-model change (i.e. it's bigger than it looked), **stop and say so** — switch to the full `requirements → design → tasks → implementation` flow instead of proceeding on the lightweight path.

---

## Learnings log

After completing any bug fix or enhancement, append an entry to `/specs/learnings.md`:

- Date, what changed (link to `/specs/<change-name>/` or the bug fix)
- Root cause (for bug fixes) or key design decision (for enhancements)
- Any baseline doc that was corrected or updated as a result
- Anything surprising or non-obvious that future work on this area should know

Keep entries short — a few lines each. This file is append-only; don't edit past entries. Always check this file (step 0 above) before starting new work — the answer to "why does this module do X" often lives here rather than in the code itself.
