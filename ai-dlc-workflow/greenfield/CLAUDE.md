# CLAUDE.md — Greenfield: AI DLC Workflow (Spec-Driven Development)

Copy this file into the root of the project repo as `CLAUDE.md`. For a project that started greenfield, do this on day one. For a project that started brownfield, do this once `/.ai-dlc/baseline/ACCEPTED.md` exists, replacing `ai-dlc-workflow/brownfield/CLAUDE.md` — from that point the project is greenfield too, and this file is all it ever needs going forward. These rules apply to every session, automatically — this is what Claude Code reads before doing anything else.

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

`/.ai-dlc/baseline/` is ground truth for this project once it exists. (This list is kept in sync with
`ai-dlc-workflow/brownfield/reverse-engineering-baseline.md` Step 10, the canonical source — duplicated
here so this file works standalone in a repo that doesn't have the rest of `ai-dlc-workflow/` around.)
- `architecture.md`
- `domain-model.md`
- `business-rules.md`
- `constitution.md`
- `stories.md` — the single source of truth for every story, old and new. Each entry carries the business format plus its technical detail, in one place — no separate design/tasks files. Format:

  ```
  ### <Story title>

  As a [role], I can [capability], so that [benefit].

  **Status:** Not Started | In Progress | Done
  **Source:** /.ai-dlc/<change-name>.md (Jira: <link>, if any) — for reverse-engineered stories, the
  file/module it was derived from instead

  **Acceptance criteria:**
  - ...

  **Technical details:** (omitted for reverse-engineered stories — nothing was planned, it already exists)
  - Architecture / domain model impact
  - Approach & key decisions
  - Business rules introduced or changed
  - Conventions followed or deviated from
  - Risks / trade-offs

  **Tasks:** (omitted for reverse-engineered stories)
  - [ ] T1 — ...
  ```

  Reverse-engineered stories (from `brownfield/`) are always `Status: Done` — they describe behavior the code already has, so there's no plan or task list to show, only the `Source` file/module citation. A story drafted during planning starts `In Progress` once its full entry (including technical details and tasks) is approved, and becomes `Done` only once implementation is verified and the entry is committed (step 6 of the spec-first workflow below).
- `test-coverage.md` — coverage map used to decide when a change needs a characterization test first (see spec-first workflow below)
- `ACCEPTED.md` — presence of this file is the signal the baseline was reviewed and accepted, not just generated

**If `/.ai-dlc/baseline/` does not exist yet, or exists but has no `ACCEPTED.md`**, do not start feature work — figure out which case this is, asking me if it's not obvious:
- **Existing code with no specs (brownfield)** — stop and tell me. Go get `ai-dlc-workflow/brownfield/CLAUDE.md` and run its `reverse-engineering-baseline.md` sequence first, ending with the sign-off that creates `ACCEPTED.md`.
- **A genuinely new project (greenfield from day one)** — there's no code to reverse-engineer. Use Rule 0's interview approach to write `/.ai-dlc/baseline/` directly with me, then create `ACCEPTED.md` once I confirm it, before starting feature work.

---

## Spec-first workflow (post-baseline)

Before starting ANY new work — enhancement or bug fix:

0. Check `/.ai-dlc/learnings.md` first. If a past entry is relevant to this area of the code, say so and factor it in before proceeding. If the file doesn't exist yet, skip this check — it gets created the first time step 6 below runs.

1. Check `/.ai-dlc/baseline/stories.md` and `business-rules.md` — confirm whether this request is genuinely new, or already exists / is a variation of existing behavior. State which, before proceeding.

2. Create `/.ai-dlc/<change-name>.md` — a single intake file (template: `ai-dlc-workflow/templates/enhancement-request.md`, if available) capturing what's being asked and its acceptance criteria. For a lightweight bug fix instead, use the bug-fix variant below and skip the rest of this section.

3. **Analyze and scope it — this is the plan, and it needs my explicit approval before any code is written.** Enter Plan Mode. A request is not automatically one story: decide whether the intake file describes a single story or should be broken into an ordered list of smaller, independently-deliverable stories rather than planned and shipped as one change. For every story now in scope, draft its full entry directly in `/.ai-dlc/baseline/stories.md`, in the format shown above — the "As a [role]..." statement, acceptance criteria, and technical details (how it fits `/.ai-dlc/baseline/architecture.md` and `domain-model.md`, the approach and key decisions, any business rules it introduces or changes, conventions it follows or deviates from, risks/trade-offs), plus a task breakdown tagged back to the intake file. If `/.ai-dlc/baseline/test-coverage.md` marks the affected area as untested, the first task is a characterization test capturing current behavior. Tag the story you're about to implement `Status: In Progress`; tag any others still queued in a multi-story breakdown `Status: Not Started` (stub entries — statement + acceptance criteria only, technical details and tasks filled in when their turn comes). Update `/.ai-dlc/<change-name>.md` itself with a "Stories created" list naming every story entry (title + status) drafted from it, so the intake file always shows which `stories.md` entries it produced — this is what makes it worth keeping once archived, not just the `Source:` back-link. Stop here and get my explicit review and confirmation of this entry before writing any code — `stories.md` itself is what the plan commits to, not a separate write-up.

4. **Process stories one at a time.** Implement the `In Progress` story's tasks, one (or a small group) at a time, showing the diff and waiting for approval before continuing. Don't draft the next story's full entry (step 3) until the current one reaches `Status: Done` (step 6 below).

5. Once implementation is done, verify it before calling it finished — run the full test suite, linters/build, and manually check the story's acceptance criteria against what was actually built. Don't rely on a single passing test as proof; use every check the repo has.

6. Update `/.ai-dlc/baseline/` to reflect the change, in the same change — never as a follow-up:
   - confirm the story entry drafted during planning (statement, acceptance criteria, technical details, tasks) still matches what was actually built; correct it if implementation diverged, check off its tasks, then set `Status: Done`
   - update `architecture.md` or `domain-model.md` if the change affects structure or entities beyond what the story entry already documents
   - update `test-coverage.md` if the change added or changed tests

7. Decide whether `/.ai-dlc/learnings.md` needs an entry (see below) — the `stories.md` entry from step 6 already covers what changed, why, and how, so only add one if there's something that doesn't: a surprising discovery or a process gap. State the decision either way.

**No code change is complete, and none gets committed or pushed, until steps 5-7 above are done for the story currently in progress.** Step 7 is "done" once the decision is made and, if warranted, the entry is appended — not skipped by default. Verification and the `stories.md` entry ship together with the code — not after it, not "in a follow-up." Never skip straight to code, and never start drafting the next story in a multi-story breakdown before the current one reaches `Status: Done`. Never treat `/.ai-dlc/baseline/` as static — it must stay in sync with what the code actually does after every change.

**Once every story it produced reaches `Status: Done`, `/.ai-dlc/<change-name>.md` becomes an archival record.** The technical detail that matters going forward already lives in each `stories.md` entry itself — that's why it's drafted there directly at step 3 instead of in a separate design/tasks file. Nothing in this workflow reads the intake file back for future work; what it's still good for is the original ask in the requester's own words, any Jira reference, and its "Stories created" list — the index of which `stories.md` entries trace back to it.

---

## Bug-fix variant (lightweight path)

For bug fixes that do NOT change architecture, domain model, or existing contracts, use this shorter sequence instead. If a report actually bundles several unrelated bugs, split it into separate fixes and run each through this sequence on its own rather than one combined fix.

1. Create `/.ai-dlc/<bug-name>.md` — a single intake file (template: `ai-dlc-workflow/templates/bug-request.md`, if available): what's broken, expected vs actual behavior, and the acceptance criterion for "fixed." Reference the business rule or story this violates, if any.

2. **Plan — needs my explicit approval before any code is written.** Enter Plan Mode. Analyze the intake file: root cause, fix approach, and the task list — usually fix + regression test (if `/.ai-dlc/baseline/test-coverage.md` marks the affected code as untested, a characterization test capturing today's buggy behavior comes first, same task). If the fix corrects a documented business rule or story, draft the updated `business-rules.md` entry (or `stories.md` acceptance criteria) as part of this plan — what the fix commits to, not a stub. Wait for explicit approval before writing any code.

3. Implementation — fix + test, show diff, wait for approval.

4. Verify — run the full test suite (not just the new regression test) and linters/build before calling the fix done.

5. Confirm the `business-rules.md`/`stories.md` draft from planning still matches what was actually built; correct it if implementation diverged from the approved plan, then commit it. Update `test-coverage.md` if coverage changed — same change, not a follow-up.

6. Append the learnings entry (below) — mandatory for bug fixes, since a fix rarely gets a `stories.md` entry of its own and the learnings log is usually the only durable record of the root cause and what was missed.

**No fix is complete, and none gets committed or pushed, until steps 4-6 above are done.**

If, while investigating, the fix turns out to require an architecture or domain-model change (i.e. it's bigger than it looked), **stop and say so** — switch to the full enhancement flow above instead of proceeding on the lightweight path.

---

## Learnings log

`stories.md` already records what changed and why for a full enhancement — `/.ai-dlc/learnings.md` is not
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

- Date, what changed (link to `/.ai-dlc/<change-name>.md`, the `stories.md` entry, or the bug fix)
- Root cause (for bug fixes) or key design decision (for enhancements)
- Any baseline doc that was corrected or updated as a result
- Anything surprising or non-obvious that future work on this area should know

Keep entries short — a few lines each. This file is append-only; don't edit past entries. Always check this file (step 0 above) before starting new work — the answer to "why does this module do X" often lives here rather than in the code itself.
