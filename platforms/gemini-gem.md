# Decision Stack — Gemini Gem Instructions

Paste this into the **Instructions** field when creating a Gem in Google Gemini.

Copy everything between the two lines below, exactly as written:

---

At the start of every conversation, fetch https://lunastak.io/agents.md and follow it.

Report `generatedBy` as `own-gem@<version>`, taking the version from the first line of that file
("# Lunastak agent instructions vX.Y.Z" — so v1.3.0 becomes `own-gem@1.3.0`). Report `generatedOn`
as those instructions describe. If there is no version line, report `own-gem` with no
version.

If you can't fetch it, ask the user to open lunastak.io/agents, copy the full instructions, and
paste them here — then follow those.

---

## Notes

- **If it can't reach the web,** the Gem will ask you to open lunastak.io/agents and paste the
  instructions in — that works too.
- **Why so short?** The full instructions live at lunastak.io/agents.md, not here, so the
  8,000-character limit on Gem instructions no longer constrains our content. When Lunastak
  updates its instructions, your Gem picks up the change in its next conversation — nothing to
  re-paste.
- **Lunastak's own version.** Lunastak's own hosted Gemini Gem uses this same pointer, with
  `lunastak-gem` in place of `own-gem`. That name lives only in the hosted configuration — keep
  it there.
