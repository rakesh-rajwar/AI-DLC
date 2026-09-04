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

`/specs/baseline/` is ground truth for this project once it exists. (This list is kept in sync with
`ai-dlc-workflow/brownfield/reverse-engineering-baseline.md` Step 10, the canonical source — duplicated
here so this file works standalone in a repo that doesn't have the rest of `ai-dlc-workflow/` around.)
- `architecture.md`
- `domain-model.md`
- `business-rules.md`
- `constitution.md`
- `stories.md` — each entry carries `Status: Not Started | In Progress | Done`. Reverse-engineered stories (from `brownfield/`) are always `Done` — they describe behavior the code already has. A story drafted during planning starts `In Progress` once its plan is approved, and becomes `Done` only once implementation is verified and the entry is committed (step 5 of the spec-first workflow below).
- `test-coverage.md` — coverage map used to decide when a change needs a characterization test first (see spec-first workflow below)
- `ACCEPTED.md` — presence of this file is the signal the baseline was reviewed and accepted, not just generated

**If `/specs/baseline/` does not exist yet, or exists but has no `ACCEPTED.md`**, do not start feature work — figure out which case this is, asking me if it's not obvious:
- **Existing code with no specs (brownfield)** — stop and tell me. Go get `ai-dlc-workflow/brownfield/CLAUDE.md` and run its `reverse-engineering-baseline.md` sequence first, ending with the sign-off that creates `ACCEPTED.md`.
- **A genuinely new project (greenfield from day one)** — there's no code to reverse-engineer. Use Rule 0's interview approach to write `/specs/baseline/` directly with me, then create `ACCEPTED.md` once I confirm it, before starting feature work.

---

## Spec-first workflow (post-baseline)

Before starting ANY new work — enhancement or bug fix:

0. Check `/specs/learnings.md` first. If a past entry is relevant to this area of the code, say so and factor it in before proceeding. If the file doesn't exist yet, skip this check — it gets created the first time step 5 below runs.

1. Check `/specs/baseline/stories.md` and `business-rules.md` — confirm whether this request is genuinely new, or already exists / is a variation of existing behavior. State which, before proceeding.

2. **Scope it.** A request is not automatically one story. Decide whether it's:
   - a lightweight bug fix (no story of its own — see the bug-fix variant below), or
   - a single story, or
   - a big feature that should be broken into an ordered list of smaller, independently-deliverable stories rather than planned and shipped as one change (and a bug report that bundles several unrelated issues gets split the same way, into separate lightweight fixes).

   If it's more than one story, write the ordered breakdown — story name + one-line scope, in delivery order (template: `ai-dlc-workflow/templates/enhancement/scope.md`, if available) — and get it approved before starting the first story's `requirements.md`. Once approved, add a stub entry to `stories.md` for each story in the breakdown, tagged `Status: Not Started`, so the full set is visible even before work on later ones begins. Then **process stories one at a time**: don't start the next story's `requirements.md` until the current one has reached `Status: Done` in `stories.md` (step 5 below complete for it).

3. For the story currently being worked, create `/specs/<story-name>/` and go through, in order (optional starting-point skeletons for these live in `ai-dlc-workflow/templates/enhancement/`, if available):
   - `requirements.md` — what's being asked, acceptance criteria. If the product team already created a Jira story/epic (or similar ticket) for this work, reference it here — it's the durable link back to product-side tracking, since `/specs/<story-name>/` itself goes dormant once the story is done (see below).
   - `design.md` — how it fits with the existing architecture and domain model in `/specs/baseline/`
   - `tasks.md` — ordered, small implementation tasks, each tagged to a requirement. If `/specs/baseline/test-coverage.md` marks the affected area as untested, add a task to write a characterization test capturing current behavior before the task that changes it.
   - **plan** — enter Plan Mode and lay out the implementation plan built from `tasks.md`, checked against `/specs/baseline/` (architecture, domain model, business rules, conventions). As part of this plan, draft the full new/updated `stories.md` entry in the same format as the rest of the file — "As a [role], I can [capability], so that [benefit]" plus the complete acceptance criteria this change commits to (drawn from `requirements.md`), tagged `Status: In Progress` — not a stub. This is what the plan commits to delivering, not a write-up invented after the fact. Once the story is written, stop and explicitly ask me to review and confirm it — separately from the rest of the plan if useful. Do not write any code until I've confirmed the story and approved the plan.
   - implementation — one task (or small task group) at a time; show the diff and wait for approval before continuing

4. Once implementation is done, verify it before calling it finished — run the full test suite, linters/build, and manually check the plan's acceptance criteria against what was actually built. Don't rely on a single passing test as proof; use every check the repo has.

5. Update `/specs/baseline/` to reflect the change, in the same change — never as a follow-up:
   - confirm the story and acceptance criteria drafted during planning still match what was actually built; correct them if implementation diverged from the approved plan, then commit that full entry to `stories.md` with `Status: Done`
   - add any new business rule to `business-rules.md`
   - update `architecture.md` or `domain-model.md` if the change affects structure or entities
   - update `test-coverage.md` if the change added or changed tests

6. Decide whether `/specs/learnings.md` needs an entry (see below) — the `stories.md` entry from step 5 already covers "what changed and why," so only add one if there's something that doesn't: a trade-off, a correction, a surprise. State the decision either way.

**No code change is complete, and none gets committed or pushed, until steps 4-6 above are done for the story currently in progress.** Step 6 is "done" once the decision is made and, if warranted, the entry is appended — not skipped by default. Verification and baseline docs ship together with the code — not after it, not "in a follow-up." Never skip straight to code, and never start the next story in a multi-story breakdown before the current one reaches `Status: Done`. Never treat `/specs/baseline/` as static — it must stay in sync with what the code actually does after every change.

**Once a story reaches `Status: Done`, `/specs/<story-name>/` becomes an archival record — nothing in this workflow reads it back for future work.** Future requests check `stories.md` and `business-rules.md` (step 1), not old story folders. So anything future work will need from a completed story — a rule, a structural change, a gotcha — must already be captured in `stories.md`, `business-rules.md`, `architecture.md`/`domain-model.md`, or `learnings.md` by the time it closes; don't rely on someone going back to re-read `requirements.md`/`design.md` later. The folder still has value as history — the detailed reasoning behind a decision, or a link to a Jira story/epic the product team created — just not as something the ongoing process itself consults.

---

## Bug-fix variant (lightweight path)

For bug fixes that do NOT change architecture, domain model, or existing contracts, skip `design.md` — use this shorter sequence instead (optional starting-point skeletons live in `ai-dlc-workflow/templates/bug-fix/`, if available). If the report actually bundles several unrelated bugs, that's a scoping call too (step 2 above) — split it into separate fixes and run each through this sequence on its own rather than one combined fix.

1. `requirements.md` (lightweight) — what's broken, expected vs actual behavior, and the acceptance criterion for "fixed." Reference the business rule or story this violates, if any.
2. `tasks.md` — usually a single task: root cause + fix + regression test. If `/specs/baseline/test-coverage.md` marks the affected code as untested, first write a characterization test capturing today's (buggy) behavior, then the regression test proving the fix — same task.
3. **plan** — enter Plan Mode with the fix plan built from `tasks.md`, checked against `/specs/baseline/`. If the fix corrects a documented business rule or story, draft the updated `business-rules.md` entry (or `stories.md` acceptance criteria) as part of the plan — what this fix commits to, not a stub — and flag it for review alongside the rest of the plan. Wait for explicit approval before writing any code.
4. implementation — fix + test, show diff, wait for approval.
5. Verify — run the full test suite (not just the new regression test) and linters/build before calling the fix done.
6. Confirm the `business-rules.md`/`stories.md` draft from planning still matches what was actually built; correct it if implementation diverged from the approved plan, then commit it. Update `test-coverage.md` if coverage changed — same change, not a follow-up. Append the learnings entry (below) — mandatory for bug fixes, since a fix rarely gets a `stories.md` entry of its own and the learnings log is usually the only record of the root cause and what was missed.

**No fix is complete, and none gets committed or pushed, until steps 5-6 above are done.**

If, while investigating, the fix turns out to require an architecture or domain-model change (i.e. it's bigger than it looked), **stop and say so** — switch to the full `requirements → design → tasks → implementation` flow instead of proceeding on the lightweight path.

---

## Learnings log

`stories.md` already records what changed and why for a full enhancement — `/specs/learnings.md` is not
a second place to say the same thing. It's for what a story entry can't hold: bug fixes (which rarely
get a story of their own), and the "why was this missed" that no acceptance criterion captures.

- **Bug fixes (bug-fix variant)** — always append an entry. This is usually the only durable record of
  a bug fix: root cause, why it wasn't caught (no test? wrong assumption? undocumented rule?), and a
  link to the fix.
- **Enhancements (full flow)** — append an entry only if there's something the `stories.md` entry
  doesn't already say: a key design trade-off, a baseline doc that needed correcting, a surprising
  discovery, or a process gap worth flagging. If the story entry covers everything that matters, skip
  it and say so — don't restate it here for the sake of completing a step.

When an entry is warranted:

- Date, what changed (link to `/specs/<change-name>/` or the bug fix)
- Root cause (for bug fixes) or key design decision (for enhancements)
- Any baseline doc that was corrected or updated as a result
- Anything surprising or non-obvious that future work on this area should know

Keep entries short — a few lines each. This file is append-only; don't edit past entries. Always check this file (step 0 above) before starting new work — the answer to "why does this module do X" often lives here rather than in the code itself.
