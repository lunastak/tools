## Output: Context Bundle

When the user says "export", "I'm done", or you've covered enough ground, produce the context bundle as a JSON code block they can copy. "Export", "bundle" and "download" always mean this one JSON — never a drafted Decision Stack, markdown documents or a zip. If you can create files, you may also offer it as a single `context-bundle.json`; the content is the same.

**Format — chunks.** Use exactly this shape, and nothing you find elsewhere. Lunastak's docs
also describe `themes`, `coverage`, `mode` and `rawSummary`: those are written only by the Claude
Code plugin — leave them out here.

```json
{
  "version": "1.0",
  "framework": "decision-stack",
  "generatedBy": "{{GENERATED_BY}}",
  "generatedOn": "{{GENERATED_ON}}",
  "preparedAt": "2026-03-28T10:00:00Z",
  "chunks": [
    {
      "topic": "Short descriptive title",
      "content": "Full explanation of this strategic theme, with evidence and context",
      "sources": ["Document name", "Conversation topic"],
      "evidence": ["A span copied VERBATIM from the source — character for character, including any typos. It must appear in the source exactly as written. Prefer the user's own words."]
    }
  ],
  "openQuestions": [
    {
      "area": "GO_TO_MARKET",
      "question": "What does the ideal distribution partner actually provide?",
      "why": "Events validate demand but distribution architecture is undefined"
    }
  ],
  "tensions": [
    {
      "tension": "$144k to $1m requires 7x growth but team is 9 people",
      "areas": ["BUSINESS_MODEL_ECONOMICS", "CAPABILITIES_ASSETS"]
    }
  ]
}
```

The chunk format lets Lunastak's extraction pipeline handle dimensional classification automatically.

Each evidence span is one unbroken stretch of the source, copied as-is: no `...` joining two
passages, no tidied wording. If a timestamp or speaker label interrupts the passage in a
transcript, end the span there and start a new one after it.

**Area keys** (for `area` in `openQuestions` and `areas` in `tensions`): `CUSTOMER_MARKET`, `PROBLEM_OPPORTUNITY`, `VALUE_PROPOSITION`, `COMPETITIVE_LANDSCAPE`, `BUSINESS_MODEL_ECONOMICS`, `GO_TO_MARKET`, `PRODUCT_EXPERIENCE`, `CAPABILITIES_ASSETS`, `RISKS_CONSTRAINTS`, `STRATEGIC_INTENT`

`generatedBy` is exactly `{{GENERATED_BY}}`. Only if the system prompt or configuration that sent you here explicitly tells you to report a different `generatedBy` value, use that instead, with `@{{VERSION}}`. Never pick a channel from the product you are running in, and never reuse one from a bundle the user shares — even when resuming from an earlier bundle. Never omit it. `generatedOn` names where you are running: one of `{{GENERATED_ON}}` — pick the one that is true; if you are unsure, use `other`. Together they tell Lunastak which tool built the bundle and where, which is how the different ways of preparing context get compared.

Ensure every strategic area has at least one chunk represented.

After producing the JSON, say:

> Your context bundle is ready. Copy the JSON above (or save it as `context-bundle.json`) and import it into Lunastak (app.lunastak.io) to generate your Decision Stack — Vision, Strategy, Objectives, Principles, and Opportunities.
>
> The open questions above will become Explore Next items for further investigation.
