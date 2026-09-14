## Output: Context Bundle

When the user says "export", "I'm done", or you've covered enough ground, produce the context bundle as a JSON code block they can copy.

**Format — chunks:**

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

**Area keys** (for `area` in `openQuestions` and `areas` in `tensions`): `CUSTOMER_MARKET`, `PROBLEM_OPPORTUNITY`, `VALUE_PROPOSITION`, `COMPETITIVE_LANDSCAPE`, `BUSINESS_MODEL_ECONOMICS`, `GO_TO_MARKET`, `PRODUCT_EXPERIENCE`, `CAPABILITIES_ASSETS`, `RISKS_CONSTRAINTS`, `STRATEGIC_INTENT`

`generatedBy` is `{{GENERATED_BY}}` unless the configuration you were given names a different channel (for example `lunastak-gpt` or `own-gpt`) — then use that name with `@{{VERSION}}`. Never omit it. `generatedOn` names where you are running: one of `{{GENERATED_ON}}` — pick the one that is true; if you are unsure, use `other`. Together they tell Lunastak which tool built the bundle and where, which is how the different ways of preparing context get compared.

Ensure every strategic area has at least one chunk represented.

After producing the JSON, say:

> Your context bundle is ready. Save this as `context-bundle.json` and import it into Lunastak (app.lunastak.io) to generate your Decision Stack — Vision, Strategy, Objectives, Principles, and Opportunities.
>
> The open questions above will become Explore Next items for further investigation.
