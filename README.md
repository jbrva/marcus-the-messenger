# Marcus and the Lost Scroll — Project Handoff

## What this is

A self-contained, browser-based educational game that teaches Roman numerals to elementary-age kids. Built as a single HTML file (~50KB) with no dependencies beyond a Google Font that loads on first open.

**Audience:** Elementary school age (roughly grades 2–5). Designed and tested with a young boy in mind, but works for any kid learning numerals.

**Time to play:** ~15–20 minutes for a full playthrough. Replayable.

## Learning design

The game weaves together three threads in every stage so the symbols stick from multiple angles:

1. **Story** — Marcus, a young Roman messenger boy, must deliver a mysterious scroll. Each stage takes him to a different real place in ancient Rome.
2. **Symbol origins** — every numeral gets a historical/etymological explanation (e.g., V = shape of an open hand, C = Latin *centum* / "century," M = Latin *mille* / "millennium," D = half of an old 1000 symbol). This is the part that makes symbols memorable instead of arbitrary.
3. **Math rules** — explicit addition rule, then the subtraction trick, then both combined.

Each stage ends with 5–6 varied practice questions (read a numeral, write a numeral, story problems, numeral arithmetic). Wrong answers trigger a one-sentence explanation showing the math broken out.

## Structure

| Stage | Place | Symbols | Reward |
|-------|-------|---------|--------|
| I | The Forum | I, V, X | The Scribe's Quill |
| II | The Aqueduct | L, C | The Engineer's Chisel |
| III | The Colosseum | IV, IX, XL, XC (subtraction) | The Gladiator's Helm |
| IV | The Emperor's Hall | D, M, CD, CM | The Emperor's Seal |
| Final | The Lost Scroll | Mixed challenges including MMXXVI (current year) and CMXCIX | Victory |

26 questions total: 21 across the four stages + 5 in the final scroll.

## Tech notes

- Single HTML file, no build step, no install.
- Vanilla JavaScript, inline SVG illustrations, embedded CSS.
- Loads Cinzel (Roman-inscription serif) and Lora from Google Fonts on first open; caches after that. Works offline thereafter.
- Won't preview correctly inside Google Drive — must be downloaded and opened locally, or hosted (GitHub Pages, Netlify Drop, etc.) for a shareable URL.
- **No save state** between sessions — closing the tab restarts progress. A short playthrough makes this acceptable, but it's the most likely first enhancement.

## Possible next steps

- Add save-progress so kids can pause mid-game.
- Harder mode with timed questions or larger numerals.
- Printable companion sheet (numeral reference + offline practice problems).
- Audio narration for emerging readers.
- More stages (e.g., reading numerals on real Roman monuments, modern uses like Super Bowl LIX or movie copyright dates).
- Spanish/Latin language toggle.

## Files

- `index.html` — the game, ready to open in any browser. (The `index.html` name also makes it drop-in deployable on GitHub Pages, Netlify Drop, etc.)
