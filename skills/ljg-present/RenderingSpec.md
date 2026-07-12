# Rendering Spec

The generation contract for `ljg-present`. `Workflows/Generate.md` reads this file before processing any presentation; the template and validator are governed by it.

## 1. Source Manifest First

Build the source manifest during parsing, before generating slides. Every source element gets a stable `SRC-NNN`, and every slide carries `sourceIds`.

Manifest order is the original document order:

1. heading
2. paragraph
3. list item
4. quote
5. table caption / table
6. example / fenced code

After generation, dedupe by each slide's first `sourceIds` reference, and this order must match the manifest order. No source element may silently disappear.

"After generation" here means after the complete HTML has had all four placeholders injected, and after `RAW_SLIDES` has been re-parsed from the final `<script>` tag. Template injection must always use a functional replacer; never pass a string containing user text directly as the replacement string for `String.replace`, or `$$`, `$&`, ``$` ``, and `$'` will be interpreted by JavaScript as replacement patterns. The in-memory object before serialization can only be used for a pre-check — it is never the final proof of fidelity.

## 2. Metadata, Cover and Chrome

| Source | Output |
|---|---|
| `#+title:` / Markdown H1 metadata | `<title>` and the cover's main content |
| `#+author:`, `#+date:`, or user-supplied meta | subtitle/meta footer |
| `#+filetags:` | theme inference |

Cover rules:

- `#+title:` always produces page 1 with `cover:true`.
- The first outline node stays on a following page.
- Only when the first node's plain text is exactly identical to the title should that node be promoted into the cover, to avoid duplication.
- The cover doesn't occupy a source-manifest ID; when merged, it retains the node's original sourceId.
- The cover explicitly uses `flex-direction: column`, `align-items:center`, `justify-content:center`; don't rely on the default row axis.
- The cover's `.fit-box` transform origin must be `center center`, so long titles remain on the page's center axis after scaling.
- For Hacker-theme covers, the reference range for short titles is `centerX = 47–53%W`, `centerY = 42–56%H`; portrait mode keeps the same center axis.
- The cover must keep at least `max(24px, 4vh)` of clear space from the footer.

Section-heading rules:

- A level-1 heading is a dark-field `emphasis` page, sharing the center axis with the cover, differentiated only by font size and content role, not by rhythm.
- A level-2-or-deeper heading is a light-paper `title` page, kept horizontally and vertically centered, with a centered short signal line no wider than `18%` of the viewport.
- Title pages strip any visual indent produced by the source heading level; depth only controls font size, never the center axis.
- Title pages don't use a top border spanning the full content width, and don't reuse the regular 2/3/4-line card grid.
- Browser-acceptance requirements: cover/title/emphasis `centerX = 47–53%W`, adjacent text-page center-axis change `≤3%W`, `centerY = 40–58%H`, `fitScale ≥ 0.80`.
- The rhythm change between heading pages comes from color field, font size, and signal line — never from an abrupt left/right position change.

Chrome rules:

- Don't create an information-carrying `<header>` or top guide.
- The footer pager exists on every page.
- The meta footer is only visible when `index === 0`; every other page shows only the pager.

## 3. Page Types

```jsonc
{
  "cover": true,
  "emphasis": true,
  "title": true,
  "depth": 2,
  "quote": true,
  "semanticGroup": "list-run",
  "sourceIds": ["SRC-001"],
  "lines": [
    {"indent": 0, "chunks": [{"t": "text"}, {"t": "emphasis", "hl": true}]}
  ]
}
```

```jsonc
{
  "preTitle": "optional",
  "pre": "ASCII / code, preserved character-for-character",
  "sourceIds": ["SRC-002"]
}
```

```jsonc
{
  "table": {
    "caption": "optional",
    "header": true,
    "rows": [["A", "B"], ["C", "D"]]
  },
  "sourceIds": ["SRC-003", "SRC-004"]
}
```

`table.header` is inferred from Org's rule-line semantics or a Markdown separator row. Never unconditionally treat the first row as a header — doing so would change the meaning of a headerless table.

Physical pagination protects both semantic completeness and projection readability: a consecutive, same-indent list with no structural boundary first forms a run. A run of 3–4 items stays together on one page; beyond 4 items, split by 3–4 per page, avoiding a lone trailing item. A whole-group page records `semanticGroup: "list-run"` for browser auditing. Only when the real effective font-size falls below 40px, or overflow occurs, should it be split further by hand — never fall back to "always two items per page." Quotes keep at most 2 original non-empty lines per page. When one source spans multiple pages, every continuation page carries:

```jsonc
{"sourceParts":[{"id":"SRC-061","index":2,"total":2,"joinBefore":"\n"}]}
```

The audit compares against the manifest in the order sourceIds first appear, and reconstructs the original visible text using `index / total / joinBefore`. Continuation pages of the same source must be contiguous with complete numbering; `allSourcesReferencedOnce` is informational only — `allSourcesReferenced` and `continuationsValid` are the actual gates.

## 4. Theme Grammar

A single presentation uses exactly one theme.

| theme | regular | cover / emphasis | hl |
|---|---|---|---|
| black | black background, white text | red background, white text | red |
| red | red background, white text | black background, white text | gold |
| yellow | yellow background, black text | black background, white text | red |
| hacker | `#EAF4EC` paper / `#07110D` text | `#07110D` field / `#EAF4EC` text | `#00C46A` |
| hacker-dark | `#06110D` field / `#CFE1D5` text | `#020806` deep field / `#CFE1D5` text | `#25E981` |

Hacker's generative grammar:

- A centered horizontal signal track establishes structure; left/right padding, decoration, and text all share the same center axis.
- The static fine grid serves only as paper texture, never as a full-screen HUD.
- Hard edges, centered short signals, and table labels may use signal green.
- ASCII/pre uses a dark hard-edged panel.
- No floating shadows, no matrix rain, no flicker, no stacked glow.

`hacker-dark` adds these constraints:

- Every page keeps a dark field: regular pages use `#06110D`, cover/emphasis use the deeper `#020806`; never fall back to light paper.
- Body text uses soft gray-green `#CFE1D5`, with contrast against the regular-page background of at least `≥9:1`; don't use pure white, which causes glare.
- 2–4-line cards use a `#0A1A13` panel; signal green is reserved for card top edges, structural tracks, emphasis, and header labels.
- The static grid may exist, but must not add glow, shadow, CRT effects, matrix rain, a blinking cursor, or any animation/transition.

## 5. Text Length and Multi-line Density

CJK characters are weighted at `1.8`, other characters at `1`.

Per-page length tiers: `≤2 single`, `≤6 short`, `≤10 medium`, `≤26 long`, everything else `xlong`. Roughly 6 CJK characters should already fall into `long`, so the oversized `medium` tier doesn't get aggressively shrunk a second time by the fit guard. Title pages are still governed by their own spatial contract.

Before length tiering, first identify semantic atoms:

- Exactly one visible line of text;
- Not a list item, and not a whole-line display formula;
- Counted with `Intl.Segmenter(..., {granularity:"grapheme"})`, `≤16` grapheme clusters after whitespace is stripped.

Semantic atoms get `data-semantic-atom=true` and `data-takahashi=true`. Their `.lines` use `width:max-content`, and `.line` is finally overridden to `white-space:nowrap`, after which measured fit scales the whole sentence proportionally. This override must come after the quote, long/xlong, and portrait rules, to guarantee that complete short sentences like "人 → 人 + Agents" ("humans → humans + agents") or "AI 为火药，人为点火者。" ("AI is the gunpowder, humans are the spark.") aren't re-split by generic line-wrap rules.

```js
const weights = slide.lines.map(lineLength);
const lineCount = slide.lines.length;
const maxWeight = Math.max(...weights);
const totalWeight = weights.reduce((sum, value) => sum + value, 0);

const density =
  maxWeight >= 45 || totalWeight >= 110 ? "dense" :
  maxWeight >= 24 || totalWeight >= 65 ? "medium" :
  "light";

const layout = lineCount >= 2 && lineCount <= 4 ? "rows" : "single";
```

Written attributes:

- slide: `data-line-count`, `data-density`, `data-layout`
- line: `data-line-index`, `data-weight`

CSS order: single-line `data-len` rules come first, multi-line rules after. Multi-line `.lines` uses a single-column grid; `.line` remains a plain inline content container.

Projection font-size floors (1098×648 landscape):

- single / short / medium Takahashi flow: effective font-size `≥90px`.
- title / emphasis: `≥78px`.
- long / xlong / quote: `≥42px`.
- 2–4-line regular text: `≥40px`.
- table: `≥30px`.

The `centerX` change between consecutive text pages must not exceed `3%W`. Portrait mode still keeps a single column and the center axis, only allowing symmetric shrinking of left/right padding, followed by re-running fit.

## 6. Pretty Wrapping

- single / short / medium content and semantic atoms prefer `nowrap`, forming the primary Takahashi-flow visual; semantic-atom detection takes priority over CJK-weighted length.
- `long`, `xlong`, quotes, and multi-line cards allow wrapping; a definite content width must be established first, so the fit guard doesn't shrink the max-content as a whole.
- For plain-text fragments that are allowed to wrap, wrap the trailing three CJK characters plus punctuation in `.keep-cjk-tail{white-space:nowrap}`; the span adds no characters, and `textContent` stays identical to the source. Short semantic atoms are already fully `nowrap` and don't rely on this tail protection.
- Use `overflow-wrap: break-word`, `word-break: normal`, `text-wrap: pretty`.
- Never use `overflow-wrap: anywhere` as a default — it produces ugly breaks in both Chinese and English text.
- Grid items must have `min-width: 0; min-height: 0`.

## 7. Measured Fit

All visible content lives inside a single `.fit-box`. Each time a page is shown:

1. Clear any previous transform.
2. Read padding on all four sides from the slide's computed style.
3. Compute `availableWidth/availableHeight`.
4. Read `.fit-box.scrollWidth/scrollHeight`.
5. `scale = min(1, availableWidth/contentWidth, availableHeight/contentHeight) * 0.975`.
6. Write `data-fit-scale` and `data-fits`.

Re-measurement is triggered by:

- `resize`
- `fullscreenchange`
- `document.fonts.ready`
- `ResizeObserver` (enabled after a capability check)
- layout changes after a portrait media-query switch

For regular non-table/pre text pages, `fitScale < 0.80` counts as a readability failure: split the page first, then consider shrinking the font. Since a semantic atom's goal is to keep a complete single line, its acceptance instead checks the final effective font-size, `computed font-size × fitScale ≥56px`; a ratio below `0.80` alone is not a failure in this case. `fits=true` only means the content doesn't overflow — it cannot substitute for the projection font-size floor.

pre/ASCII effective font-size is accepted by physical line count: `≤16` lines `≥22px`, `17–24` lines `≥18px`, `25–28` lines `≥15.5px`. The panel as a whole is centered, with characters left-aligned inside it; beyond 28 lines or below the floor, split the diagram by hand instead of mechanically truncating the character art.

## 8. Offline Math

Only closed delimiters are parsed; an inline opening `$` must not be immediately followed by a digit or a question mark:

- display: a complete `$$...$$`, or a whole-line `$...$`
- inline: a closed `$...$` within a sentence

Minimum supported commands:

- `\cdot`, `\times`, `\propto`
- `\alpha`, `\beta`, `\gamma`
- `^{...}` / `^x`
- `_{...}` / `_x`

Unrecognized commands are left as-is. `$20/month`, `$200/month`, `$???/month` must remain plain text, even when several prices appear on the same line — they must never be paired together as a formula. Math expressions starting with a digit should use `$$...$$`.

## 9. Motion and Interaction

The whole piece is hard-cut throughout. Forbidden:

- CSS `animation*` / `transition*` / `view-transition*` / `@keyframes`
- `scroll-behavior: smooth`
- JS `.animate()` / `setInterval()`
- a blinking cursor or autoplay

`requestAnimationFrame` is allowed only for layout measurement and post-paint fit, never for visual motion.

Key mapping:

| Action | Keys |
|---|---|
| next | ArrowRight, ArrowDown, Space, Enter, j, PageDown |
| prev | ArrowLeft, ArrowUp, k, PageUp |
| first / last | Home / End |
| fullscreen | f / F |

If an event originates from an `input`, `textarea`, `select`, or `contenteditable=true` element, the page-turn listener must return immediately, so the deck doesn't hijack editing keys when embedded in an interactive shell.

## 10. Verification

Static validation rejects resource tags, `@import`, CSS `url(...)`, and `image-set(...)` alike, to ensure the single file is genuinely offline:

```bash
bun Tools/ValidateDeck.ts <html> --theme <theme>
```

Visual verification: use Interceptor's isolated test context to check at least:

- the cover
- the spatial rhythm of level-1 emphasis and level-2 title pages; all three keep the same horizontal center axis
- a regular single-line page
- every `data-semantic-atom=true` page: each `.line`'s Range rect has a line count of 1, with effective font-size meeting the corresponding projection floor
- every `data-semantic-group=list-run` page: 3–4 items complete on one page, order unchanged, effective font-size `≥40px`
- the longest single paragraph page
- the highest-density page among the 2/3/4-line pages
- the largest table
- the largest ASCII/pre
- every formula variant
- one landscape and one portrait size each

Collect computed style and bounding boxes page-by-page in a real browser: outside of table/pre content, text must be `text-align:center`; content center must fall within `47–53%W`; regular non-table/pre pages need `fitScale ≥0.80` with zero overflow; semantic atoms are instead checked for final effective font-size `≥56px` and a visual line count of 1; all other font sizes must satisfy this spec's projection floors. Table/pre pages are accepted against their own structural and density gates.

When Interceptor isn't available, keep the static evidence and explicitly mark it deferred; don't substitute other browser automation or the main browser.
