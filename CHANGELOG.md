# Changelog

All notable changes to the `lunastak` plugin are recorded here.

The version below is the plugin version declared in `.claude-plugin/marketplace.json` and
`.claude-plugin/plugin.json`. **Bump it on every published change** — Claude Code caches an
installed plugin by version, so users who already installed the previous version keep their
stale copy of any changed file, including `SKILL.md`, until the version moves.

### Release checklist

The version lives in `.claude-plugin/plugin.json` only; the build stamps it into every generated
file and refuses to run if `marketplace.json` disagrees.

1. Bump `version` in `.claude-plugin/plugin.json` **and** `.claude-plugin/marketplace.json`.
2. `node scripts/build.mjs` — regenerates `skills/decision-stack/SKILL.md` and `dist/agents.md`.
3. `node scripts/build.mjs --check` — must print "all outputs current" (CI runs it too).
4. `claude plugin validate .`
5. Bump `CURRENT_INSTRUCTIONS` in app.lunastak.io (`src/lib/import/instructions-version.ts`) — until
   the app reads www.lunastak.io's `/agents/version.json` instead (agents design, step 4).
6. app.lunastak.io PR #43 (new channel names + legacy map) is deployed to **production** — otherwise
   new-named bundles are stored as `unknown`.
7. Release together with www.lunastak.io's `/agents` (it serves `dist/agents.md` at
   `https://www.lunastak.io/agents.md`) — the thin pointers fetch it. Always the `www` host: the
   bare `lunastak.io` 307-redirects to it, and some agent fetchers don't follow cross-host redirects.

Never hand-edit the two generated files: edit `src/`, then build.

## [Unreleased]

## [1.3.0] — 2026-09-14

### Changed
- **One source, one build.** The instructions now live once, in `src/`, and
  `node scripts/build.mjs` builds both published files from them, stamping the version from
  `.claude-plugin/plugin.json`. `--check` fails when either output is stale, and CI runs it on
  every push. The plugin skill and every chat route can no longer drift apart.
- **The platform templates are thin pointers.** `platforms/claude-project.md`, `custom-gpt.md` and
  `gemini-gem.md` now tell the assistant to fetch `https://www.lunastak.io/agents.md` and follow it (or
  ask the user to paste it), so a self-built assistant picks up every update without re-pasting,
  and the GPT's 8,000-character instruction limit no longer constrains the content.
- **Channels are named by owner, and every channel is versioned.** `generatedBy` is
  `<channel>@<version>`: `lunastak-skill`, `lunastak-agents`, `lunastak-gpt`, `lunastak-gem`
  (Lunastak hosts or controls it) and `own-gpt`, `own-gem`, `own-claude-project` (the user built it
  from a template). They replace `claude-code-plugin`, `custom-gpt-published`,
  `gemini-gem-published`, `custom-gpt`, `gemini-gem` and `claude-project` — the legacy names are
  still accepted by app.lunastak.io from the step-1 PR (#43) onwards, and map to the new channels.
  The hosted GPT and Gem carry `lunastak-gpt` / `lunastak-gem` in their own configuration only.
- **In Claude Code, `agents.md` offers the plugin for next time** — shows the two install
  commands, waits for a yes, and carries on inline in this session either way.
- **Coverage levels match the bundle format.** Gap analysis listed "thin" as a coverage level,
  which was never valid and produced invalid bundles in the wild; it now uses `rich` /
  `adequate` / `partial` / `empty`, like the coverage display and the bundle format.
- **`agents.md` says up front that Lunastak writes the Decision Stack** and the assistant's only
  output is the JSON bundle; "export" means that JSON, never documents or a zip. Evidence spans
  are one unbroken stretch — no `...` joins, and they stop at a transcript's timestamp or speaker
  label. The one-paste line now reads "…follow it to help me gather context for Lunastak" rather
  than "…prepare my Decision Stack". A written summary is not the deliverable either: the
  document route's summary is a line or two per area, and a user who has already asked for the
  bundle gets the JSON straight away. (In live tests ChatGPT first drafted a Decision Stack and
  exported a zip of markdown files, then wrote a long prose "context" document and called it the
  thing to import; Gemini stitched spans across timestamps.) The chat format says to use exactly its shape — `themes`, `coverage`, `mode` and `rawSummary` belong to the plugin — and the checklist requires `generatedBy` (ChatGPT had copied the plugin shape from the public spec page and dropped it).
- **`agents.md` asks the assistant to open with "Using Lunastak agent instructions v<version>."** so
  the user can tell it read the file rather than searching the web for Lunastak (Gemini did, and
  drafted a Decision Stack from the marketing pages).
- The README no longer promotes skills.sh (its listing is a passive crawl of an old snapshot); it
  leads with the one paste instead.

### Added
- **`dist/agents.md`** — the instructions for any assistant, built from the same source as the
  skill: routing (Claude Code vs everyone else), ground rules, the shared core, and the
  chunks-only output. Line 1 is `# Lunastak agent instructions v<version>`, which the pointers
  read. www.lunastak.io serves it at `/agents.md` — every pointer uses the `www` host, since the
  bare domain redirects across hosts and some agent fetchers won't follow that.
- **`generatedOn`** — an optional, self-reported field naming where the bundle was made
  (`claude-code`, `codex`, `cursor`, `chatgpt`, `claude.ai`, `gemini`, `other`). Analytics only.
  It fixes the case where a skills.sh install in another harness reported itself as the Claude
  Code plugin.

## [1.2.0] — 2026-09-09

### Added
- **Bundles now say which tool made them.** Every format emits `generatedBy` — the plugin as
  `claude-code-plugin@1.2.0`, and each platform template as its own value. Lunastak validates it
  against a closed set and records it against every fragment, so the different ways of preparing
  context can finally be compared.

  Until now no bundle was attributable: the Claude Project, Custom GPT and Gemini Gem emit
  byte-identical bundles and were indistinguishable from each other, not merely unrecorded.

  The two **published** assistants carry `custom-gpt-published` / `gemini-gem-published`, which
  live only in their own hosted configuration and never in this repo. That one string is the only
  thing separating a hosted assistant from a self-built one — regenerating either from its
  template would silently erase the distinction. Both platform files now carry a warning saying so.


## [1.1.0] — 2026-09-09

### Changed
- **Every theme and chunk now carries a verbatim evidence span** — copied character for
  character from the source, typos included, preferring the user's own words. Applied across
  the skill, `/lunastak:export`, the bundle format doc, and all three platform variants
  (Claude Project, Custom GPT, Gemini Gem), so a bundle means the same thing whichever one
  produced it.

  This pairs with app.lunastak.io v2.7.0, which verifies each span against its source at
  import. A paraphrase cannot be verified, so it lands as `unverifiable` and the ground truth
  review cannot show the user the words behind it. **Bumping the version is what makes this
  reach people** — an installed plugin is cached by version, so anyone on 1.0.1 keeps the old
  `SKILL.md` until this number moves.

### Added
- `LICENSE` (MIT) — the README already linked it, but the file was missing.

## [1.0.1] — 2026-08-26

### Changed
- README links the [skills.sh listing](https://skills.sh/lunastak/tools/decision-stack) as an
  alternative browse/install route.
- Version bumped so installed copies of the plugin refresh from the marketplace.

## [1.0.0] — 2026-05-21

### Added
- Restructured the repo as a Claude Code plugin: `lunastak:decision-stack` skill plus the
  `/lunastak:decision-stack`, `/lunastak:export`, and `/lunastak:resume` commands.
- Marketplace manifest so `claude plugin install lunastak@lunastak-tools` works end-to-end.
- README onboarding for first-time Claude Code users.

### Earlier (pre-plugin, 2026-04-08)
- Initial publish of the decision-stack skill and the Custom GPT / Gemini Gem / Claude Project
  platform variants.
