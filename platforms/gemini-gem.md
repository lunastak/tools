# Decision Stack — Gemini Gem Instructions

A Gem can't open links, so it carries Lunastak's instructions as a knowledge file.

1. Download the instructions: open https://www.lunastak.io/agents.md and save the page as
   `agents.md`.
2. Create a Gem in Google Gemini. Under **Knowledge**, upload `agents.md`.
3. Paste this into the **Instructions** field — everything between the two lines below, exactly as
   written:

---

Follow the Lunastak agent instructions in the attached knowledge file, agents.md, for the whole conversation.

Report `generatedBy` as `own-gem@<version>`, taking the version from the first line of that file
("# Lunastak agent instructions vX.Y.Z" — so v1.3.0 becomes `own-gem@1.3.0`). Report `generatedOn`
as those instructions describe.

If the attached file is missing, ask the user to open https://www.lunastak.io/agents, copy the full
instructions, and paste them here — then follow those.

---

## Notes

- **Check it worked.** The Gem's first reply starts "Using Lunastak agent instructions v1.3.0."
  (or whichever version you uploaded).
- **Keeping it current.** Your Gem stays on the version you uploaded. When
  https://www.lunastak.io/agents shows a newer version, download `agents.md` again and replace the
  file under **Knowledge**. If Lunastak tells you on import that newer instructions are available,
  this is the fix.
- **Why not a link?** A regular Gemini chat can open https://www.lunastak.io/agents.md, but a Gem
  can't, and it asks you to paste the instructions instead. The knowledge file saves that step.
- **Lunastak's own version.** Lunastak's own hosted Gemini Gem uses these same instructions, with
  `lunastak-gem` in place of `own-gem`. That name lives only in the hosted configuration — keep
  it there.
