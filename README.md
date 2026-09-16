# deckforge

Agent-first presentation studio for field engineering. Local, headless, with a measured preflight.

Customer decks (discovery readouts, architecture walkthroughs, pilot results) are now mostly written by an agent. An agent will overflow a slide or assert a number nobody measured. deckforge gives the agent a CLI and an MCP server with 19 operations over a versioned deck library: create, edit one slide, checkpoint, review, render, export. Every mutation carries the deck's current revision. A stale writer is rejected instead of clobbering work. `deck.render` opens the deck in headless Chromium and measures every slide for overflow, so a text box that spills past the frame fails preflight. A small browser UI exists for when a human wants to look. Nothing depends on it.

Decks are version 1 JSON: six layouts, three themes, up to 30 slides. Each claim is labelled evidence, assumption or proposal. Four skills ship in `skills/` for the recurring deck types (discovery narrative, technical architecture, pilot readout, deck review), each with a template deck, evaluation prompts and a format reference.

## Quick start

Node 22 or newer.

```sh
npm ci
node agent/cli.mjs commands                                 # every operation with its input schema
node agent/cli.mjs init --workspace /absolute/path/decks    # explicit; never created implicitly
node agent/cli.mjs setup --workspace /absolute/path/decks   # prints an MCP server config
node agent/cli.mjs ui --workspace /absolute/path/decks      # optional review UI on loopback
npm run browser:install                                     # Chromium, needed only for deck.render
```

Operations take JSON on `--input file.json` or stdin and answer with `{ ok, data | error }` on stdout. `--output` never overwrites an existing file. `--read-only` drops every mutating operation from the MCP tool list.

`npm test` runs 59 tests, including a headless render.

## What it produces

- A `workspace.json` per library, written atomically, with archives and up to 12 named checkpoints per deck.
- Offline HTML presentations with no network requests and no speaker notes.
- PDF and per-slide PNG from the headless renderer, plus a preflight report tied to the revision it measured.
- A brief ZIP: agent brief, deck JSON and the full skill folder, framed so the deck content is treated as untrusted text.

## In the suite

deckforge is one of three tools in [fieldpack](https://github.com/ong6/fieldpack). [proofpack](https://github.com/ong6/proofpack) hands pilot evidence to deckforge for a readout deck. The deck skills here are what [skillforge](https://github.com/ong6/skillforge) versions and evaluates.

## More from ong6

Forges make things, packs bundle them.

- [groundplane](https://github.com/ong6/groundplane) — fails the build when an agent asserts a fact its tools never produced
- [jobforge](https://github.com/ong6/jobforge) — grades the interview plan you say out loud, not the code you submit
- [skillforge](https://github.com/ong6/skillforge) — skill discovery, versioning and baseline-aware evaluation
- [proofpack](https://github.com/ong6/proofpack) — pilot evidence, review proposals and customer-safe handovers
- [fieldpack](https://github.com/ong6/fieldpack) — deckforge, skillforge and proofpack as one local-first suite
- [skillpack](https://github.com/ong6/skillpack) — the Claude Code and Codex skills used across all of these
- [uipack](https://github.com/ong6/uipack) — React and SVG figure components behind the diagrams on junxiong.dev
