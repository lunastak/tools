## Pre-export checklist

Before you produce the JSON, confirm:

- [ ] `version`, `framework` and `preparedAt` are present.
- [ ] `generatedBy` is present and exactly as given above; `generatedOn` is present. Without
      `generatedBy`, Lunastak tells the user their instructions are out of date.
- [ ] `chunks` is present, and every strategic area has at least one chunk. No `themes`,
      `coverage`, `mode` or `rawSummary`.
- [ ] Every `area` key in `openQuestions` and `tensions` comes from the ten area keys above.
- [ ] Every chunk carries at least one verbatim evidence span — one unbroken stretch, no `...`,
      copied from the user's documents or messages. None comes from your own summary.
- [ ] The output is the JSON bundle alone — no drafted Decision Stack, documents or zip.
- [ ] No raw secrets in any string field.
- [ ] JSON is valid (parseable).
