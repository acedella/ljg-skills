# ValidateDeck

Deterministically checks whether a single-file HTML deck produced by `ljg-present` still satisfies the presentation contract.

## Usage

```bash
bun Tools/ValidateDeck.ts <deck.html> --theme hacker
bun Tools/ValidateDeck.ts SloganTemplate.html --template --theme hacker
bun Tools/ValidateDeck.ts --self-test
bun Tools/ValidateDeck.ts --help
```

## Options

| Option | Meaning |
|---|---|
| `--theme <name>` | Runs extra checks for the active theme; `hacker` verifies the three colors and the light/dark reading strategies |
| `--template` | Substitutes the four template placeholders with a built-in fixture before checking, while also confirming the placeholders still exist |
| `--json` | Outputs machine-readable results |
| `--self-test` | Verifies a compliant template, rejects motion/center-axis regression fixtures, and covers the rows layout plus formula/price boundaries |
| `--help` | Shows help |

## Exit Codes

| Code | Meaning |
|---:|---|
| 0 | All checks passed |
| 1 | At least one contract violation |
| 2 | Argument error or missing input file |

## What It Checks

- Template version and JavaScript syntax.
- The document-title cover, with no duplicate-merge path.
- The cover explicitly uses a column-axis for horizontal/vertical centering, scaling from the center.
- Every line-based page shares the centered-text contract; level-2-and-deeper titles use a centered short signal line.
- The medium text tier caps out at a weighted length of 10, preventing headings of roughly 6 CJK characters or longer from first being over-enlarged and then extremely shrunk by the fit guard.
- No-information header; the meta footer appears only on the cover, while the pager appears on every page.
- Three renderers — lines, table, pre; a table only generates a header row when `header:true`.
- 2–4-line pages unify on rows, with density-composite font sizing and a portrait center axis.
- Takahashi-flow markers, xlong wrap font sizes, `min-width:0`, and the measured-fit guard.
- Offline math, protection for price strings, ASCII line-count tiering, and table projection font sizes.
- Runtime exposure of sourceParts continuation-page provenance.
- Common Bluetooth-remote-clicker arrow keys, PageUp/PageDown, and key-protection while in an input/edit state.
- Zero CSS/JS motion (covering property families and smooth scroll), plus zero resource tags, `@import`, `url(...)`, and `image-set(...)`.
- The Hacker theme's three colors, its light-body/dark-section strategy, and center-axis-symmetric decoration.

## Boundary

This tool verifies static structure and doesn't substitute for a real browser. Font fallback, actual line-wrapping, visual rhythm, and each page's final `fitScale` still need to be checked in an isolated Interceptor browser.
