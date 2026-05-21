# Lunastak

Tools for preparing strategic context to feed [Lunastak](https://app.lunastak.io) — an AI coach that turns your strategic thinking into a Decision Stack.

## Install (Claude Code)

```bash
claude plugin install lunastak/tools
```

## Skills

| Skill | Description |
|---|---|
| `lunastak:decision-stack` | Prepare a Decision Stack context bundle from documents and conversation. |

## Commands

| Command | Description |
|---|---|
| `/lunastak:decision-stack` | Start a session — bring docs, or answer guided questions. |
| `/lunastak:export` | Produce the JSON context bundle. |
| `/lunastak:resume` | Continue from a saved bundle. |

## What you produce

A [context bundle](https://lunastak.io/docs/context-bundles) — import to [app.lunastak.io](https://app.lunastak.io) to generate your Decision Stack.

## Other platforms

[Gemini Gem](./platforms/gemini-gem.md) · [Custom GPT](./platforms/custom-gpt.md) · [Claude Project](./platforms/claude-project.md)

## License

MIT
