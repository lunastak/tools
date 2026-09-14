## Output: Context Bundle

When the user says "export", "I'm done", or you've covered enough ground, produce the context bundle.

**Format:** A single JSON code block the user can copy-paste or save as a file.

```json
{
  "version": "1.0",
  "framework": "decision-stack",
  "generatedBy": "{{GENERATED_BY}}",
  "generatedOn": "claude-code",
  "preparedAt": "2026-03-28T10:00:00Z",
  "mode": "context_dump | exploration | deep_dive | gap_analysis",
  "coverage": {
    "CUSTOMER_MARKET": { "level": "rich", "sourceCount": 3 },
    "PROBLEM_OPPORTUNITY": { "level": "adequate", "sourceCount": 2 }
  },
  "themes": [
    {
      "area": "CUSTOMER_MARKET",
      "theme": "Short theme title",
      "evidence": ["A span copied VERBATIM from the source — character for character, including any typos. It must appear in the source exactly as written. Prefer the user's own words over the assistant's."],
      "confidence": "HIGH"
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
  ],
  "rawSummary": "Plain text summary of everything captured, suitable for human reading"
}
```

**Area keys:** `CUSTOMER_MARKET`, `PROBLEM_OPPORTUNITY`, `VALUE_PROPOSITION`, `COMPETITIVE_LANDSCAPE`, `BUSINESS_MODEL_ECONOMICS`, `GO_TO_MARKET`, `PRODUCT_EXPERIENCE`, `CAPABILITIES_ASSETS`, `RISKS_CONSTRAINTS`, `STRATEGIC_INTENT`

### Alternative: Chunk-based format

If the user asks for a generic format (or you're unsure which dimensions apply), use `chunks` instead of `themes`. Lunastak will handle dimensional tagging automatically.

```json
{
  "version": "1.0",
  "framework": "decision-stack",
  "generatedBy": "{{GENERATED_BY}}",
  "generatedOn": "claude-code",
  "preparedAt": "2026-03-28T10:00:00Z",
  "chunks": [
    {
      "topic": "Short descriptive title",
      "content": "Full explanation of this strategic theme, with evidence and context",
      "sources": ["Document name", "Conversation topic"],
      "evidence": ["A span copied VERBATIM from the source — character for character, including any typos. It must appear in the source exactly as written. Prefer the user's own words."]
    }
  ],
  "openQuestions": [...],
  "tensions": [...]
}
```

The `chunks` format is simpler to produce and lets Lunastak's proprietary dimensional analysis handle classification. Use `themes` (with area keys) when you're confident in the dimensional mapping; use `chunks` when the themes don't map cleanly to a single dimension.

`generatedBy` is exactly `{{GENERATED_BY}}` — copy it as written, never change or omit it. `generatedOn` names where you are running: `claude-code`, `codex`, `cursor`, or `other` — pick the one that is true; if you are unsure, use `other`. Together they tell Lunastak which tool built the bundle and where, which is how the different ways of preparing context get compared.

After producing the JSON, say:

> Your context bundle is ready. Save this as `context-bundle.json` and import it into Lunastak (app.lunastak.io) to generate your Decision Stack — Vision, Strategy, Objectives, Principles, and Opportunities.
>
> The open questions above will become Explore Next items for further investigation.
