# Bug report — <bug-name>

Single intake file — copy to `/.ai-dlc/requests/<bug-name>.md`. Claude analyzes it and proposes the fix plan
(root cause, approach, tasks) directly for approval — see `greenfield/CLAUDE.md`'s bug-fix variant. No
separate design/tasks files: the plan lives in the approval step, and the durable record afterward is
`/.ai-dlc/learnings.md` (and `/.ai-dlc/stories.md`/`business-rules.md` only if this fix corrects
something already documented there).

## Source
[Jira ticket link, if the product team or support already logged one, e.g. PROJ-1234. Optional — leave
as "none" if this came directly from investigation.]

## What's broken
[...]

## Expected behavior
[...]

## Actual behavior
[...]

## Violates
[The business rule or story this contradicts, if any — reference `/.ai-dlc/business-rules.md`
or `stories.md`. Leave "none identified" if this is genuinely undocumented behavior.]

## Acceptance criterion for "fixed"
[A single, specific, testable criterion.]
