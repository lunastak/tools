# Context bundle format

The context bundle is the JSON artefact produced by `/lunastak:export` in the plugin, or by any assistant following [www.lunastak.io/agents.md](https://www.lunastak.io/agents.md) — including Lunastak's hosted GPT and Gem and the platform templates in `platforms/`. It travels from that tool into [Lunastak](https://app.lunastak.io), which uses it to generate a Decision Stack.

The canonical version of this spec lives at [lunastak.io/docs/context-bundles](https://www.lunastak.io/docs/context-bundles). This file mirrors it for offline reference.

## Top-level shape

```json
{
  "version": "1.0",
  "framework": "decision-stack",
  "generatedBy": "<channel>@<version>",
  "generatedOn": "claude-code | codex | cursor | chatgpt | claude.ai | gemini | other",
  "preparedAt": "2026-05-21T10:00:00Z",
  "mode": "context_dump | exploration | deep_dive | gap_analysis",
  "coverage": { ... },
  "themes": [ ... ],
  "openQuestions": [ ... ],
  "tensions": [ ... ],
  "rawSummary": "Plain text summary of everything captured."
}
```

| Field | Required | Type | Notes |
|---|---|---|---|
| `version` | yes | string | Schema version. Current: `"1.0"`. Bumped only when the JSON shape changes — not the instructions version, which travels in `generatedBy`. |
| `framework` | yes | string | Always `"decision-stack"`. |
| `generatedBy` | yes (new bundles) | string | Which tool produced the bundle, as `<channel>@<instructions-version>`. See below. Bundles emitted before 2026-09-09 lack it; Lunastak still imports them but tells the user their instructions are out of date. |
| `generatedOn` | optional | string | Where it was made: `claude-code`, `codex`, `cursor`, `chatgpt`, `claude.ai`, `gemini` or `other`. Self-reported, analytics only. Added in 1.3.0. |
| `preparedAt` | yes | string (ISO 8601) | When the bundle was emitted. |
| `mode` | skill route | string | Dominant interaction shape. One of `context_dump`, `exploration`, `deep_dive`, `gap_analysis`. |
| `coverage` | skill route | object | Coverage per strategic area. See below. |
| `themes` | one of | array | Structured themes tagged by area. Use this OR `chunks`. |
| `chunks` | one of | array | Untagged content blocks. Use when dimensional mapping is unclear. |
| `openQuestions` | optional | array | Questions surfaced for further exploration. |
| `tensions` | optional | array | Contradictions or trade-offs noted during the session. |
| `rawSummary` | skill route | string | Human-readable summary of the whole session. |

**skill route** = the plugin skill always emits it; the `agents.md` route (chunks only) does not.
Lunastak's importer treats all three as optional — the one thing it rejects is a bundle with
neither a non-empty `themes` nor a non-empty `chunks`.

## `generatedBy` — which tool made this bundle

Optional, added 2026-09-09 (1.2.0); channels renamed and every channel versioned in 1.3.0.
Backwards compatible: absent is valid, and every bundle emitted before 2026-09-09 lacks it.

The value is `<channel>@<instructions-version>`, where the version is the tools release
(`.claude-plugin/plugin.json`) the instructions came from. The channel names **who configured the
assistant**: `lunastak-*` = Lunastak hosts or controls it; `own-*` = the user set it up from one of
our templates.

| Channel | Emitted by |
|---|---|
| `lunastak-skill@<version>` | anything running the skill file — the Claude Code / Desktop plugin, or a skills.sh install in another harness (`generatedOn` says which) |
| `lunastak-agents@<version>` | an assistant that read [www.lunastak.io/agents.md](https://www.lunastak.io/agents.md) (or its pasted text) directly |
| `lunastak-gpt@<version>` | **Lunastak's hosted Custom GPT** |
| `lunastak-gem@<version>` | **Lunastak's hosted Gemini Gem** |
| `own-gpt@<version>` | a Custom GPT the user built from `platforms/custom-gpt.md` |
| `own-gem@<version>` | a Gem the user built from `platforms/gemini-gem.md` |
| `own-claude-project@<version>` | a Claude Project the user built from `platforms/claude-project.md` |

The `lunastak-gpt` / `lunastak-gem` names live ONLY in the two hosted assistants' own
configuration, never in this repo's templates. That is deliberate and it is the only way a hosted
assistant can be told from a self-built one — the distinction cannot be inferred from bundle
content, and a verbatim copy of the hosted configuration would claim `lunastak-*` too.
**Regenerating a hosted assistant from its template would silently turn it into `own-*`.**

### Legacy names (before 1.3.0)

Bundles made with older instructions still carry these, and stored rows already hold them.
Lunastak (app.lunastak.io) keeps accepting them and maps each to its new channel, so nothing
already recorded becomes `unknown`.

| Legacy value | Now |
|---|---|
| `claude-code-plugin@<version>` | `lunastak-skill` |
| `custom-gpt-published` | `lunastak-gpt` |
| `gemini-gem-published` | `lunastak-gem` |
| `custom-gpt` | `own-gpt` |
| `gemini-gem` | `own-gem` |
| `claude-project` | `own-claude-project` |

Lunastak validates against this closed set and stores anything else as `unknown`; the value is
self-reported by a model, so it is never trusted as free text. Absent is stored as null, which
means "unknown" and is not a category.

### `generatedOn` — where it was made

Optional, added in 1.3.0: the harness the bundle was made in. One of `claude-code`, `codex`,
`cursor`, `chatgpt`, `claude.ai`, `gemini`, `other`. The skill offers `claude-code | codex | cursor
| claude.ai | other`; `agents.md` offers all seven. Like `generatedBy` it is self-reported and used
for analytics only — anything outside the set is stored as `unknown`.

## Which route emits which format

Two sets of instructions produce bundles, and they are **not equivalent**. This is deliberate, and
worth knowing before you compare two bundles and wonder why one is richer. Both are built from the
same source in `src/` — `skills/decision-stack/SKILL.md` and `dist/agents.md`.

| Route | Channels | Emits | Dimensions assigned by | Notes |
|---|---|---|---|---|
| **Plugin skill** (`lunastak:decision-stack`) | `lunastak-skill` | `themes` **and** `chunks` | the tool, at capture — `area` + `confidence` per theme | The fullest. Also the only route with `/lunastak:resume`. |
| **`agents.md`** — any assistant handed https://www.lunastak.io/agents.md; Lunastak's hosted GPT and Gem; a Claude Project, Custom GPT or Gem built from `platforms/` (the GPT and Project templates point at `agents.md`; the Gem attaches it) | `lunastak-agents`, `lunastak-gpt`, `lunastak-gem`, `own-gpt`, `own-gem`, `own-claude-project` | `chunks` | Lunastak, by an LLM tagging pass at import | Gems can't fetch URLs, so a Gem carries `agents.md` as a knowledge file. |

Both shapes are first-class: `import-bundle` picks the direct area mapping when a bundle has no
`chunks`, and the LLM tagging pass when it does. A `chunks` bundle costs one extra LLM call at
import and has its dimensions **inferred** rather than captured; a `themes` bundle carries the
tagging the user actually saw.

### Why the `agents.md` route stops at `chunks`

Two reasons, and only one of them was ever a hard limit.

**The deliberate one.** `chunks` hands dimensional classification to Lunastak, which tags with the
same analyser it uses for conversations and documents. A GPT or a Gem is not Claude, and its
guess at which of ten areas a theme belongs to is the weakest link in the chain. Letting the app
tag keeps every chat route producing identical bundles, and keeps the tagging consistent with
everything else in a project.

**The ceiling — gone since 1.3.0.** A ChatGPT Custom GPT caps its Instructions field at **8,000
characters**, and the field truncates silently. When each platform template carried the full
instructions, the dimensional format (the area keys, the `confidence` scale, and the guidance for
choosing between the two shapes) did not fit. The templates are now short pointers to
`agents.md`, so the limit no longer constrains our content.

**So if the `agents.md` route should emit `themes`, that is a product decision rather than a
technical one** — and because every chat route now follows the one file, it is taken for all of
them at once. 1.3.0 deliberately did not change it: that release moved where the instructions live
and what the channels are called, not what the bundles contain.

## Strategic area keys

Coverage and themes use these ten keys:

`CUSTOMER_MARKET` · `PROBLEM_OPPORTUNITY` · `VALUE_PROPOSITION` · `COMPETITIVE_LANDSCAPE` · `BUSINESS_MODEL_ECONOMICS` · `GO_TO_MARKET` · `PRODUCT_EXPERIENCE` · `CAPABILITIES_ASSETS` · `RISKS_CONSTRAINTS` · `STRATEGIC_INTENT`

## Coverage

```json
"coverage": {
  "CUSTOMER_MARKET":      { "level": "rich",     "sourceCount": 3 },
  "PROBLEM_OPPORTUNITY":  { "level": "adequate", "sourceCount": 2 },
  "VALUE_PROPOSITION":    { "level": "partial",  "sourceCount": 1 },
  "COMPETITIVE_LANDSCAPE":{ "level": "empty",    "sourceCount": 0 }
}
```

`level` is one of `rich`, `adequate`, `partial`, `empty`.

## Themes (preferred when areas map cleanly)

```json
"themes": [
  {
    "area": "CUSTOMER_MARKET",
    "theme": "Short theme title",
    "evidence": ["A span copied VERBATIM from the source — character for character, including any typos. It must appear in the source exactly as written. Prefer the user's own words over the assistant's."],
    "confidence": "HIGH"
  }
]
```

`confidence` is `HIGH`, `MEDIUM`, or `LOW`.

## Chunks (use when areas are ambiguous)

```json
"chunks": [
  {
    "topic": "Short descriptive title",
    "content": "Full explanation of this strategic theme, with evidence and context.",
    "sources": ["Document name", "Conversation topic"],
    "evidence": ["A span copied VERBATIM from the source — character for character, including any typos. It must appear in the source exactly as written. Prefer the user's own words."]
  }
]
```

Lunastak applies dimensional analysis automatically when `chunks` is used.

## Open questions

```json
"openQuestions": [
  {
    "area": "GO_TO_MARKET",
    "question": "What does the ideal distribution partner actually provide?",
    "why": "Events validate demand but distribution architecture is undefined."
  }
]
```

These become Explore Next items in Lunastak.

## Tensions

```json
"tensions": [
  {
    "tension": "$144k to $1m requires 7x growth but team is 9 people.",
    "areas": ["BUSINESS_MODEL_ECONOMICS", "CAPABILITIES_ASSETS"]
  }
]
```

## Secrets

Bundles must not contain secrets. The `decision-stack` skill enforces redaction before any user-supplied text is included. Replace any detected secret with `[REDACTED:<kind>]` and note that a secret was redacted. See the **Secret Redaction** section of `skills/decision-stack/SKILL.md` (the same text is in `dist/agents.md`) for the full list.

## Validation checklist

Before emitting, confirm:

- [ ] `version`, `framework` and `preparedAt` are present.
- [ ] `generatedBy` and `generatedOn` are present.
- [ ] Either `themes` or `chunks` is present (or both), and not empty.
- [ ] Skill route only: `mode`, `coverage` and `rawSummary` are present.
- [ ] All `area` and `coverage` keys come from the ten strategic-area keys above.
- [ ] Every theme/chunk carries at least one verbatim evidence span.
- [ ] No raw secrets in any string field.
- [ ] JSON is valid (parseable).

`dist/agents.md` carries a chat-adapted copy of this list (from `src/agents-checklist.md`): chunks
only, no `mode` / `coverage` / `rawSummary`.
