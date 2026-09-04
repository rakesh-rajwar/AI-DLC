# Scope — <feature-name>

Only needed when a request breaks into more than one story (step 2 of the spec-first workflow). Get
this breakdown approved before starting the first story's `requirements.md`. Once approved, add a stub
entry to `/specs/baseline/stories.md` for each story below, tagged `Status: Not Started`.

## Why this is more than one story
[What made this too big for a single story — scope, risk, natural seams in the work.]

## Story breakdown (delivery order)

1. **<story-1-name>** — [one-line scope]
2. **<story-2-name>** — [one-line scope]
3. **<story-3-name>** — [one-line scope]

Stories are processed one at a time: `/specs/<story-name>/` isn't created for the next story until the
current one reaches `Status: Done` in `stories.md`.
