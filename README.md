# Lunastak

Tools for preparing strategic context to feed [Lunastak](https://app.lunastak.io) — an AI coach that turns your strategic thinking into a Decision Stack.

---

## Why this exists

[Lunastak](https://app.lunastak.io) is an AI coach that builds a [Decision Stack](https://thedecisionstack.com) — a five-layer strategic artefact from vision through execution. The app generates one from a guided conversation.

But conversation isn't always the right starting point. If you've already done the thinking — in decks, transcripts, planning sessions, half-finished memos — you don't want to recap it from memory. You want to feed Lunastak what you've already got.

These instructions do that. They extract and organise strategic context from whatever you bring — in ChatGPT, Claude, Gemini, Claude Code or any other assistant — then export it as a JSON [context bundle](https://lunastak.io/docs/context-bundles) you import into [app.lunastak.io](https://app.lunastak.io).

---

## Quick start — any assistant

Paste this into a new conversation:

> Read lunastak.io/agents.md and follow it to help me prepare my Decision Stack

That's it — nothing to install. The assistant fetches the current instructions, works through your material with you, and hands you the bundle to import. (If it can't open links, open [lunastak.io/agents](https://lunastak.io/agents), copy the instructions, and paste them in.)

## Claude Code plugin

In Claude Code you can install the plugin instead — it adds `/lunastak:export` and `/lunastak:resume`, and tags themes by strategic area as it goes. Add the marketplace, install the plugin:

```bash
claude plugin marketplace add lunastak/tools
claude plugin install lunastak@lunastak-tools
```

In any Claude Code session:

```
/lunastak:decision-stack
```

Or describe what you want — the skill auto-triggers on phrases like *"prep my decision stack"*, *"build a context bundle"*, or *"organise my strategic thinking"*.

---

## First time using Claude Code?

If you haven't installed Claude Code yet, the path is short.

1. Install Claude Code: [code.claude.com](https://code.claude.com).
2. Open a terminal anywhere.
3. Run the install commands above.
4. Start a session with `claude` and type `/lunastak:decision-stack`.

Try this as a first prompt:

> *"I want to prep a Decision Stack context bundle. I've got a vision doc and the transcript from our last quarterly planning session."*

The skill opens by asking what you're bringing, then extracts strategic material organised by the [ten strategic areas](https://lunastak.io/docs/decision-stack). When coverage is sufficient, run `/lunastak:export` to produce the bundle.

---

## What are skills and commands?

**Skills** are domain knowledge. They teach Claude what a Decision Stack is, how strategic material is structured, and how to extract context without straying into advising. Auto-loaded — Claude matches your wording against the skill's description.

**Commands** are workflows you invoke. Three of them: start a session, export the bundle, resume from a saved bundle.

---

## What's inside

### Skills

| Skill | Description |
|---|---|
| `lunastak:decision-stack` | Prepare a Decision Stack context bundle from documents and conversation. |

### Commands

| Command | Description |
|---|---|
| `/lunastak:decision-stack` | Start a session — bring docs, or answer guided questions. |
| `/lunastak:export` | Produce the JSON context bundle. |
| `/lunastak:resume` | Continue from a saved bundle. |

---

## What you produce

A [context bundle](https://lunastak.io/docs/context-bundles) — JSON containing the extracted strategic material, tagged by area. Import at [app.lunastak.io](https://app.lunastak.io) and Luna generates a Decision Stack from it.

---

## Build your own assistant

Want a Gem, Custom GPT or Claude Project that always starts ready? Each template is a short pointer to [lunastak.io/agents.md](https://lunastak.io/agents.md), so your assistant picks up every update on its own:

- [Gemini Gem](./platforms/gemini-gem.md)
- [Custom GPT](./platforms/custom-gpt.md)
- [Claude Project](./platforms/claude-project.md)

---

## Contributing / releasing

The instructions have one source. Edit the files in `src/`, then run:

```bash
node scripts/build.mjs          # writes skills/decision-stack/SKILL.md and dist/agents.md
node scripts/build.mjs --check  # fails if either is stale (CI runs this)
```

**Never hand-edit `skills/decision-stack/SKILL.md` or `dist/agents.md`** — they are generated, and the next build overwrites them. The version comes from `.claude-plugin/plugin.json`; the release checklist is at the top of [CHANGELOG.md](./CHANGELOG.md).

---

## License

[MIT](./LICENSE) — fork it, modify it, ship it.

---

*Decision Stack model by [Martin Eriksson](https://thedecisionstack.com). Lunastak app and plugins by [Jonny Schneider](https://linkedin.com/in/jonnyschneider) at [Humble Ventures](https://humventures.com.au).*
