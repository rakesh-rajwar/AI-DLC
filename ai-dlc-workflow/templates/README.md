# Templates

Optional starting-point skeletons for the single-file intake docs `ai-dlc-workflow/greenfield/CLAUDE.md`
describes. Copy the relevant one and fill it in — they exist to keep the intake shape consistent, not to
replace judgment or Rule 0.

- **`enhancement-request.md`** — copy to `/specs/<change-name>.md` for anything genuinely new or
  changed. Claude analyzes this file in Plan Mode, scopes it into one story or an ordered list of
  smaller ones, and drafts each story's full entry — business format plus technical detail (design,
  tasks) — directly into `/specs/baseline/stories.md`, tagged `Status: Not Started | In Progress |
  Done`. There's no separate `design.md`/`tasks.md`/`stories-entry.md` — the story entry in
  `stories.md` holds all of it. The intake file gets a "Stories created" list added back into it, then
  becomes archival once every story it produced reaches `Status: Done`.

- **`bug-request.md`** — copy to `/specs/<bug-name>.md` for the lightweight bug-fix variant. Claude
  analyzes it in Plan Mode (root cause, fix approach, tasks) directly as the approval step — no separate
  file for that either. `/specs/learnings.md` is the durable record afterward (mandatory for bug fixes),
  plus `stories.md`/`business-rules.md` if the fix corrects something already documented there.

Both assume `/specs/baseline/` already exists and has been accepted — see `ai-dlc-workflow/brownfield/`
or `ai-dlc-workflow/greenfield/CLAUDE.md` if it doesn't yet.
