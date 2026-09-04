# Optional — Narrowing Stories with High-Level Business Context

Everything in `reverse-engineering-baseline.md` is derived purely from code. That's necessary but sometimes too broad — a story like *"As a user, I can export data, so that [PURPOSE UNCLEAR]"* is technically complete but not useful for prioritizing new work.

If you have real business context (who the product serves, current priorities, what's being sunset), feeding a short brief in **narrows and sharpens** the story set. It never replaces the code-derived truth.

**Run this only after `reverse-engineering-baseline.md` Step 8 is complete.** It refines what already exists in the baseline — it does not generate new claims.

```
Here is high-level business context for this product:
- Who it's for: [primary user types / customer segments]
- Core value proposition: [one or two sentences]
- Current priorities: [what's actively being invested in]
- Known deprecated areas: [anything being phased out]

Using this context, revisit /specs/baseline/stories.md:
1. Resolve as many [PURPOSE UNCLEAR] tags as this context allows.
2. Tag each story with a priority (core / supporting / legacy-only)
   based on this context.
3. Do not invent stories that aren't backed by code — this context is
   for interpreting and prioritizing what you already found, not adding
   new claims.
```

If you don't have this context yet, or don't want to narrow the story set, skip this file entirely — the baseline stands on its own as ground truth for future spec-first work.
