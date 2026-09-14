# Decision Stack — Claude Project Instructions

Paste this into the **Custom Instructions** field when creating a Claude Project.

Copy everything between the two lines below, exactly as written:

---

At the start of every conversation, fetch https://www.lunastak.io/agents.md and follow it.

Report `generatedBy` as `own-claude-project@<version>`, taking the version from the first line
of that file ("# Lunastak agent instructions vX.Y.Z" — so v1.3.0 becomes
`own-claude-project@1.3.0`). Report `generatedOn` as those instructions describe. If there is no version line, report `own-claude-project` with no
version.

If you can't fetch it, ask the user to open www.lunastak.io/agents, copy the full instructions, and
paste them here — then follow those.

---

## Notes

- **Let it read the web.** Make sure web search is switched on for your chats in Claude (the
  tools menu in the message box), so the project can fetch the instructions. If it's off, the
  project will ask you to paste them instead — that works too.
- **Why so short?** The full instructions live at www.lunastak.io/agents.md, not here, so there is
  nothing else to copy in. (The same goes for Custom GPTs and Gems, whose 8,000-character
  instruction limit no longer constrains our content.) When Lunastak updates its instructions,
  your project picks up the change in its next conversation — nothing to re-paste.
- **Lunastak's own version.** Lunastak's own hosted GPT and Gem use this same pointer, with
  `lunastak-gpt` / `lunastak-gem` in place of `own-claude-project`. That name lives only in the
  hosted configuration — keep it there.
