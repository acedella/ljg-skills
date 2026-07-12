---
name: ljg-present
description: "Presentation Forge (演讲铸造器, Outline-Faithful). Renders an orgmode/markdown outline 1:1 into a single-file offline HTML deck; supports black/red/yellow themes plus a light hacker theme and a dark hacker-dark theme, with automatic title cover, multi-line density layout, tables, ASCII, LaTeX, responsive sizing, and remote-clicker support. USE WHEN the user asks for '讲这个' (present this), 'present', '做成演讲' (turn this into a presentation), 'slides', '标语流' (slogan flow), '宣言体' (manifesto style), 'slogan', 'manifesto', '按 outline 美化' (beautify by outline). NOT FOR content distillation, rewriting, or corporate-style decks."
user_invocable: true
version: "4.4.0"
---

# ljg-present: Presentation Forge (演讲铸造器)

Forge the outline into a stage. The author decides the content; the skill only decides how it's seen.

## Core Contract

**The outline is the truth. The skill is the renderer.**

- Headings, paragraphs, list items, and quotes: not a word changed.
- Tables: structure unchanged; example/code blocks: whitespace and line breaks unchanged.
- All source elements appear in their original order — no extraction, no condensing, no reordering.
- The only permitted change is physical pagination and visual composition.
- `#+title:` is the document title and must first produce a standalone cover; the first outline node still gets its own following page. If the two are textually identical, they may be merged into a single cover — never duplicated.

## Workflow Routing

| Workflow | Trigger | File |
|---|---|---|
| **Generate** | present this, present, turn into a presentation, slides, beautify by outline, generate an HTML presentation | `Workflows/Generate.md` |

When generating, read `RenderingSpec.md` first, then use the root-level `SloganTemplate.html`. Don't recreate the template from memory.

## Quick Reference

### Input and Output

- Input: Orgmode, Markdown, or plain text.
- Output: `~/Downloads/{title}.html` — a single offline file with no external resource links.
- First screen: the document-title cover.
- Spatial rhythm: all text pages share a stable center axis; the cover and section pages are distinguished by dark/light color fields, font size, and a centered short signal line.
- Header: carries no information.
- Footer: the first page shows the page number plus subtitle/meta; every other page shows only the page number.

### Theme

Priority: explicit argument > `#+filetags:` > default `black`.

| Argument | theme | tone |
|---|---|---|
| `-b` / `--theme=black` | black | contemplative, argumentative |
| `-r` / `--theme=red` | red | manifesto, rallying cry |
| `-y` / `--theme=yellow` | yellow | ironic, alarming |
| `--hacker` | hacker | reverse-engineering lab paper |
| `--cyber` | hacker | compatibility alias; no longer generates CRT/HUD |
| `--theme=hacker-dark` | hacker-dark | low-glare dark terminal; every page uses a dark field with soft gray-green body text |

Hacker has two static reading variants. Both reject stacked neon effects:

```css
--hacker-void:   #07110D;
--hacker-paper:  #EAF4EC;
--hacker-signal: #00C46A;

--hacker-dark-bg:     #06110D;
--hacker-dark-deep:   #020806;
--hacker-dark-panel:  #0A1A13;
--hacker-dark-fg:     #CFE1D5;
--hacker-dark-signal: #25E981;
```

`hacker`'s regular pages use light lab paper; every page of `hacker-dark` uses a dark field, with the cover and level-1 sections pushed one shade darker still. Dark-mode body text is not pure white but a soft gray-green; signal green is reserved for the centered signal track, emphasis, and table labels. No matrix rain, glowing outlines, fake HUDs, or blinking cursors.

### Outline Mapping

| Source | Page |
|---|---|
| `* Level-1 heading` | gets its own emphasis section page |
| `**` and deeper headings | gets its own title page; the deeper the level, the smaller the font size |
| Paragraph | theme text page; physically split into further pages only when necessary |
| List | 3–4 consecutive same-level items are kept together on one page whenever possible; longer lists are split into pages of 3–4 items, avoiding a lone trailing item |
| Table | table page; split across pages beyond 6 rows, with the header row repeated |
| Quote | quote page; continues onto further pages beyond 2 original lines, recording sourceParts |
| `#+begin_example` / fenced code | pre page, preserved character-for-character |
| `*emphasis*` / `~code~` / `=verbatim=` | `hl: true`; emphasis pages ignore inline hl |

### Multi-line Is Not a Uniform Font-size Drop

Multi-line pages weigh both "line count" and "text density," but always use a single-column `rows` layout:

- 2, 3, and 4 lines are all stacked vertically along the page's center axis; the reading path never switches sides across pages.
- Font size is set by a combined "line count + light/medium/dense" rule: split pages first, enlarge second, and only then let the fit guard make fine adjustments.
- Single-line content that is not a list item, not a whole-line formula, and has `≤16` grapheme clusters after whitespace is stripped is first classified as a "semantic atom" (语义原子): even if its CJK-weighted length falls into the `long` tier, it still stays on one line as a full sentence and enters the Takahashi flow (高桥流).
- Consecutive same-level list items preserve the semantic block first: 3–4 items stay together on one page; beyond 4 items, split into groups of 3–4, avoiding stranding the last item alone.
- Grid items must have `min-width: 0` so body text wraps naturally; don't set `.line` to flex/grid, or it will break apart highlights and formulas.
- Single-line single/short/medium content uses the "Takahashi flow" (高桥流): fewer characters become the primary visual, with a landscape effective font-size target of `≥90px`.

Thresholds and DOM fields are authoritative in `RenderingSpec.md`.

### Formulas, ASCII, and Sizing

- Only closed `$...$` / `$$...$$` delimiters count as formulas; prices like `$20/month` are not formulas.
- Common symbols and sub/superscripts render offline, without relying on MathJax/CDN.
- ASCII/pre content is tiered by physical line count: `≤16` lines start at 22px, `17–24` lines at 18px, `25–28` lines at 15.5px; the panel itself is centered, with characters left-aligned inside it.
- Regular long text/quotes target an effective font-size `≥42px`, 2–4-line text `≥40px`, and tables `≥30px`; when a page falls short, split it further rather than shrink it.
- Every page measures its real available width and height, listening for resize, fullscreen, font-ready, and ResizeObserver events.
- `data-fits=true` only proves nothing overflows; for regular non-table/pre text pages, `fitScale < 0.80` requires re-splitting the page. Since semantic atoms must keep a complete single line, they're instead gated on a final effective font-size `≥56px`, so a raw font-size ratio no longer causes a false failure.

## General Interaction

- `→` `↓` `Space` `Enter` `j` `PageDown`: next page.
- `←` `↑` `k` `PageUp`: previous page.
- `Home` / `End`: first / last page.
- `f` / `F`: fullscreen.
- Swipe left/right on touchscreen, or tap the left/right half of the screen: page navigation.

Both arrow keys and PageUp/PageDown are kept, since different Bluetooth remote clickers send different key codes.

## Acceptance Gate

After writing the HTML, run:

```bash
bun Tools/ValidateDeck.ts ~/Downloads/<deck>.html --theme <theme>
```

The validator is responsible for the static contract: template version, JS syntax, title cover, header/footer, zero motion, formula protection, multi-line layout, fit guard, page types, page-turn keys, and external resource links.

Visual judgment must be re-verified in an isolated browser using Interceptor, checking representative pages and high-density pages. If an isolated context isn't available, report "static validation passed, browser visual re-verification not yet done" — don't fall back to the main browser or other screenshot tools, and don't claim visual verification has occurred.

## Gotchas

- **Visual centering isn't just `text-align:center`.** Text alignment, left/right padding, decorative tracks, and transform origin must all share the same center axis, or pages will still drift when turned.
- **Cover, emphasis, and title are three spatial roles on the same axis.** They create rhythm through font size, dark/light fields, and short signal lines — never by changing left/right anchors.
- **The scale origin is also composition.** Text pages uniformly use `center center`; otherwise, fitting will re-skew content that was originally centered.
- **Theme isn't just a color alias.** Pure black on pure white tires viewers over a long deck; dark Hacker uses deep green-black, soft gray-green text, and two shades of dark field — information hierarchy comes from structural lines and brightness contrast, not from the amount of neon effect.
- **"It fits on the page" isn't "readable from the back row."** Multi-line pages can't just shrink the font size based on the longest string; lists should preferentially keep 3–4-item semantic blocks, quotes are capped at two lines, and anything below the projection font-size floor should continue onto another page.
- **Grammatical length isn't semantic length.** A short sentence like "AI 为火药，人为点火者。" ("AI is the gunpowder, humans are the spark.") must be treated as a complete semantic atom first, even if it lands in the `long` tier after CJK weighting — generic line-wrap rules must not fling its trailing characters onto the next line.
- **The pagination unit isn't a fixed two items.** A run of 3–4 consecutive same-heading, same-level items often forms a comparison or an argument; keep the whole group on one page first, then decide whether manual splitting is needed based on real effective font-size and overflow.
- **Chinese sentence-ending text needs orphan protection.** For long sentences that are allowed to wrap, wrap the trailing three CJK characters plus punctuation in a tail span that doesn't change `textContent`, to prevent the last character from being stranded alone; don't inject hidden characters that would pollute copy-paste output.
- **`vmin` isn't responsive.** A fixed font size can only be an estimate; the real boundary must be computed from `scrollWidth/scrollHeight` together with the available width and height.
- **`fits` doesn't mean readable.** Extreme shrinking can still yield `fits=true`; regular text pages with `fitScale < 0.80`, or content below the projection font-size floor, should be re-split into more pages. Semantic atoms are accepted based on their final effective font-size alone, because their whole purpose is to scale the full sentence proportionally onto one line.
- **ASCII's ceiling is set by its line count.** A 28-line character diagram can't hit 22px on a 648px-tall screen; use the density-tiered physical floor, and split the diagram manually if needed.
- **Formula detection requires closed delimiters.** Otherwise a `$` inside a price, currency, or file path gets misread as math.
- **Multi-line grids need `min-width: 0`.** Without it, long words or formulas will push a column outside the viewport.
- **`.line` stays an inline container.** Setting it to flex/grid would break apart chunks, inline math, and highlights — layout should act on `.lines` instead.
- **Header and footer are different contracts.** The header carries no information; meta only appears in the cover's footer, while the pager appears on every page.
- **All visual motion is forbidden.** Don't just check the shorthand — also cover `animation-*`, `transition-*`, `view-transition-*`, smooth scroll, `.animate()`, and timers.
- **Offline checks can't just scan `<img>` and `https://`.** CSS-relative `url(...)`, `@import`, and `image-set(...)` can just as easily leave a single-file deck missing resources on another machine.
- **Real browser evidence can't be substituted.** The static validator prevents structural regressions, but it can't prove fonts, line-wrapping, and visual rhythm hold up in an actual Chrome window.
- **Placeholder injection must use a functional replacer.** `String.replace(pattern, replacementString)` interprets replacement patterns like `$$`, `$&`, ``$` ``, and `$'`, which can silently corrupt LaTeX or body text; inject all four template placeholders with `() => value`.
- **The fidelity audit must cover the final HTML.** Auditing only the in-memory slides before serialization misses drift introduced at the injection layer; before writing the file, re-parse `RAW_SLIDES` from the complete final HTML and re-run the same audit against the source manifest, visible text, continuations, and examples.

## Examples

### Example 1: A regular outline presentation

```text
User: Use ljg-present to present this "/Users/jjin/Documents/Obsidian Vault/Writing Notes/talk.md"
→ Read the Generate workflow, RenderingSpec, and SloganTemplate
→ Keep the entire outline, generating a title cover and black/red/yellow theme pages
→ Run ValidateDeck, then output ~/Downloads/<title>.html
```

### Example 2: A static Hacker presentation

```text
User: Turn this org file into Hacker style, no motion
→ Choose --hacker, with regular pages on a light field and section pages on a dark field
→ All text pages keep the center axis; short sentences use the Takahashi flow, and multi-line pages use a unified rows layout, splitting pages first
→ Verify zero motion, formulas, footer, responsiveness, and remote-clicker support
```

### Example 3: A static dark-Hacker presentation

```text
User: Make this all dark Hacker style, no motion of any kind
→ Choose --theme=hacker-dark, with every page using a deep green-black field and soft gray-green body text
→ Push the cover/emphasis pages one shade darker still; signal green is used only for structural lines, emphasis, and labels
→ Verify body-text contrast ≥9:1, zero shadows/motion, and zero overflow at both sizes
```

## Output Language

Default to the same language as the source outline content; if the source is already in another language and the user asks to keep it as-is, don't translate it.
