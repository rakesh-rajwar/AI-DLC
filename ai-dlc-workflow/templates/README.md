# Templates

Optional starting-point skeletons for the docs `ai-dlc-workflow/greenfield/CLAUDE.md` creates under
`/specs/<change-name>/` for each enhancement, bug fix, or new baseline. Copy the relevant set in and
fill it in — they exist to keep the shape consistent across changes, not to replace judgment or Rule 0.
Skip a section that doesn't apply rather than leaving placeholder text in a real spec doc.

- **`enhancement/`** — for anything genuinely new or changed: `scope.md` (only when the request breaks
  into more than one story), then per story the full `requirements.md → design.md → tasks.md → plan
  (stories-entry.md) → implementation` sequence from the spec-first workflow. Stories are processed one
  at a time — `stories-entry.md`/`stories.md` carry `Status: Not Started | In Progress | Done`.
- **`bug-fix/`** — for the lightweight bug-fix variant: `requirements.md → tasks.md → plan →
  implementation`, no `design.md`.

Both sets assume `/specs/baseline/` already exists and has been accepted — see
`ai-dlc-workflow/brownfield/` or `ai-dlc-workflow/greenfield/CLAUDE.md` if it doesn't yet.
