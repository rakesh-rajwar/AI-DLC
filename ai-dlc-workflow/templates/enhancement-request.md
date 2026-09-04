# Change request — <change-name>

Single intake file — copy to `/ai-dlc/requests/<change-name>.md`. This is all you write by hand; Claude analyzes
it and drafts the story (or stories) directly into `/ai-dlc/stories.md` — see
`greenfield/CLAUDE.md`'s spec-first workflow. This file becomes archival once every story it produced
reaches `Status: Done` — nothing reads it back after that.

## Source
[Jira story/epic link, if the product team already created one, e.g. PROJ-1234. Optional — leave as
"none" if this originated directly from conversation or code investigation.]

## What's being asked
[Plain-language description, in your own words — not a copy of the raw request.]

## Why
[The problem this solves or the value it delivers. Reference real business context if any exists.]

## In scope
- [...]

## Out of scope
- [...]

## Acceptance criteria
- [ ] [Criterion 1 — specific, testable]
- [ ] [Criterion 2]
- [ ] [...]

## Open questions
[Anything still unresolved — apply Rule 0: ask rather than assume. Remove this section once resolved.]

## Stories created
[Filled in during the plan step (step 3 of the spec-first workflow) — every story entry in
`/ai-dlc/stories.md` drafted from this request, title + status, e.g.:
- <story title> — Status: Done
- <story title> — Status: In Progress
Empty until then.]
