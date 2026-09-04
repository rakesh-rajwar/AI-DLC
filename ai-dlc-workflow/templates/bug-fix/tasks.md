# Tasks — <bug-name>

Usually one task.

- [ ] Root cause: [...]
- [ ] Fix: [...]
- [ ] Regression test: [...]

If `/specs/baseline/test-coverage.md` marks the affected code as untested, first write a
characterization test capturing today's (buggy) behavior, then the regression test proving the fix —
same task.

## If this turns out bigger than a bug fix

If, while investigating, the fix turns out to require an architecture or domain-model change, stop and
say so — switch to the full `enhancement/` template set (`requirements → design → tasks`) instead of
proceeding on this lightweight path.

## Plan-time draft (if this fix corrects a documented rule or story)

If the fix corrects a documented business rule or story, draft the updated `/specs/baseline/
business-rules.md` entry (or `stories.md` acceptance criteria) here as part of the plan — what this fix
commits to, not a stub — and flag it for review alongside the rest of the plan. Confirm it still matches
what was actually built after implementation, correcting if it diverged, then commit it to the baseline
docs in the same change.

[Draft entry, or "not applicable — no documented rule/story affected."]
