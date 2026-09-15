# Decision Stack — Gemini Gem Instructions

A Gem can't open links, so it carries Lunastak's instructions as a knowledge file.

1. Download the instructions: open https://www.lunastak.io/agents.md and save the page as
   `agents-v<version>.txt`, taking the version from its first line — e.g. `agents-v1.3.1.txt`.
   Gem Knowledge rejects `.md` uploads; `.txt` works and the contents are the same.
2. Create a Gem in Google Gemini. Under **Knowledge**, upload that file.
3. Paste this into the **Instructions** field — everything between the two lines below, exactly as
   written:

---

Follow the Lunastak agent instructions in the attached knowledge file (named agents-v<version>, e.g. agents-v1.3.1.txt) for the whole conversation. If more than one is attached, follow the highest version.

Report `generatedBy` as `own-gem@<version>`, taking the version from the first line of that file
("# Lunastak agent instructions vX.Y.Z" — so v1.3.0 becomes `own-gem@1.3.0`). Report `generatedOn`
as those instructions describe.

If no such file is attached, ask the user to open https://www.lunastak.io/agents, copy the full
instructions, and paste them here — then follow those.

---

## Notes

- **Check it worked.** The Gem's first reply starts "Using Lunastak agent instructions v1.3.1."
  (or whichever version you uploaded).
- **Keeping it current.** A Gem keeps its own copy of a knowledge file — even one attached from
  Google Drive doesn't update when the file changes. So your Gem stays on the version you
  uploaded, and the file name tells you which. When https://www.lunastak.io/agents shows a newer
  version, download it as `agents-v<new version>.txt`, remove the old file under **Knowledge** and
  upload the new one. The instructions above don't change. If Lunastak tells you on import that
  newer instructions are available, this is the fix.
- **Why not a link?** A regular Gemini chat can open https://www.lunastak.io/agents.md, but a Gem
  can't, and it asks you to paste the instructions instead. The knowledge file saves that step.
- **Lunastak's own version.** Lunastak's own hosted Gemini Gem uses these same instructions, with
  `lunastak-gem` in place of `own-gem`. That name lives only in the hosted configuration — keep
  it there.
