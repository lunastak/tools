---
description: Continue from a saved context bundle.
---

Ask the user to paste their existing context bundle JSON, or point you to a file path. Validate it against the schema in `docs/bundle-format.md` — if required fields are missing, ask the user to fix or re-export before continuing.

Once loaded, hydrate the session state:

1. Show coverage by area from the bundle's `coverage` field.
2. Surface any `openQuestions` and `tensions` that were captured previously.
3. Summarise what's already strong vs. thin in two or three lines.

Then offer three paths:

- **Fill gaps** — work through the thin/empty areas with one question at a time.
- **Focused deep-dive** — pick one area to go deeper on.
- **Continue** — keep extracting in whatever direction the user wants.

Follow the `decision-stack` skill for the rest of the session. When the user is ready to produce an updated bundle, run `/lunastak:export`.
