# Decision Stack — Custom GPT Instructions

Paste this into the **Instructions** field when creating a Custom GPT in ChatGPT.

Copy everything between the two lines below, exactly as written:

---

At the start of every conversation, fetch https://lunastak.io/agents.md and follow it.

Report `generatedBy` as `own-gpt@<version>`, taking the version from the first line of that file
("# Lunastak agent instructions vX.Y.Z" — so v1.3.0 becomes `own-gpt@1.3.0`). Report `generatedOn`
as those instructions describe. If there is no version line, report `own-gpt` with no
version.

If you can't fetch it, ask the user to open lunastak.io/agents, copy the full instructions, and
paste them here — then follow those.

---

## Notes

- **Let it read the web.** Under **Capabilities**, tick **Web Search** so the GPT can fetch the
  instructions. If it's off, the GPT will ask you to paste them instead — that works too.
- **Why so short?** The full instructions live at lunastak.io/agents.md, not here, so the
  8,000-character limit on GPT instructions no longer constrains our content. When Lunastak
  updates its instructions, your GPT picks up the change in its next conversation — nothing to
  re-paste.
- **Lunastak's own version.** Lunastak's own hosted Custom GPT uses this same pointer, with
  `lunastak-gpt` in place of `own-gpt`. That name lives only in the hosted configuration — keep
  it there.
