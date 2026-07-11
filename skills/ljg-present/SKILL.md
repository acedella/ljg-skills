---
name: ljg-present
description: "Presentation forge (Outline-Faithful). Renders orgmode/markdown outline hierarchy 1:1 into a visual presentation — color-block big type, ultra-bold staggered layout, the original text untouched, only beautified. Three theme colors black/red/yellow (default black, or inferred from filetags), can be explicitly overridden with -r/-b/-y; --cyber switches to a black-background green-text cyber-hacker style. Use when user says '讲这个' (present this), 'present', '做成演讲' (turn into a presentation), '呈现一下' (present/render this), '铸成演示' (forge into a deck), '做个 slides' (make some slides), '标语流' (slogan flow), '宣言体' (manifesto style), 'slogan', 'manifesto', '按 outline 美化' (beautify by outline). Outputs a single HTML file to ~/Downloads/."
user_invocable: true
version: "3.0.0"
---

# ljg-present: Presentation Forge

Forge an outline into color blocks — a visual renderer that hands the stage back to the person speaking.

## What this is NOT

- **Not a manifesto extractor** — it doesn't distill "the one line," doesn't write "complete assertive sentences," doesn't reorder content
- **Not Takahashi-style** — it doesn't whittle text down to single words
- **Not deck-style** — it's not the tidy layout of corporate PPT

## What this IS

**Outline → visual renderer**:

- Input = an orgmode file (`*` `**` hierarchy + lists + tables + emphasis)
- Output = visually beautified slogan-style HTML, **preserving the outline structure 1:1**
- No extraction, no rewriting, no condensing — it only decides **how to render this line/section as a page**

Visual language (aesthetic reference: Felipe Franco / BIG STUDIOS manifesto aesthetic):

- **One theme color for the whole piece** — pick one of red/black/yellow
- **Left-aligned stage aesthetic** — text left-aligned, oversized type naturally fills the screen
- **Oversized ultra-bold type** — single characters at 70vmin, long sentences at 11vmin
- **Multi-line staggering** — auto-indent 0/1/2 based on outline nesting depth
- **Keywords auto-colored** — `*emphasis*` `~code~` auto-highlighted
- **Section transitions set the beat** — level-1 headings `*` → emphasis cover pages, everything else → theme pages

## Core philosophy

**The outline is the truth. The skill is the renderer.**

Leaving content untouched is an iron rule:
- **Headings: not a word changed**
- **Paragraphs: not a word changed**
- **List items: not a word changed**
- **Tables: structure not changed**
- **Order: not reshuffled**

The only "movement" allowed is **physical pagination** (splitting an overly long section across multiple pages), while keeping visual consistency.

## Orgmode → page mapping rules

### Heading hierarchy

| Org element | Page |
|---|---|
| `* Level-1 heading` | Gets its own **emphasis** cover page (accent background) |
| `** Level-2 heading` | Gets its own **theme** page (large title takes a whole page) |
| `*** Level-3 heading`+ | Gets its own theme page (one size smaller) |

### Content elements

| Org element | Page behavior |
|---|---|
| Paragraph | theme page, split by period/line break/character count |
| `- List item` | theme page, one item per line, indent by nesting depth (0/1/2) |
| `1. Numbered list` | same as above, keeps the number prefix |
| Nested list | child items indent +1 (max indent=2) |
| `\| table \|` | one or more pages, preserves table structure (header row bolded) |
| `*emphasis*` | auto `hl: true` |
| `~code~` or `=verbatim=` | auto `hl: true` |
| Keywords inside `「」` | visual unit (keep the brackets, hl not forced) |
| Quote `> ...` | theme page, shown at indent 1 |
| Divider `-----` | standalone emphasis pause page (no content, pure color block) |
| `#+begin_example` block | standalone pre page (monospace-rendered ASCII art) |

### File-level metadata

| Org element | Purpose |
|---|---|
| `#+title:` | → JSON `title` (browser tab) |
| `#+author:` or `#+date:` | → JSON `subtitle` (bottom-right footer) |
| `#+filetags:` | used to infer theme (see below) |
| `#+identifier:` | ignored |

### Theme inference

**Priority**: explicit argument > filetags inference > default black

Explicit override (argument):
- `-r` / `--theme=red` → red
- `-b` / `--theme=black` → black
- `-y` / `--theme=yellow` → yellow
- `--cyber` → cyber-hacker (black background, green text + CRT scanlines + HUD + terminal cursor)

Automatic filetags inference:

| filetags contains | theme | tone |
|---|---|---|
| `:share:` `:talk:` `:manifesto:` `:keynote:` | `red` | manifesto, rallying cry |
| `:essay:` `:think:` `:learn:` `:note:` | `black` | contemplative, argumentative |
| `:critique:` `:warn:` `:rant:` | `yellow` | ironic, alarming |
| none of the above | `black` | default contemplative tone |

### Pagination rules (when content is long)

**Iron rule: after splitting, keep visual consistency. Pages from the same logical block should share the same font-size tier / background / indent rules.**

| Case | Split method |
|---|---|
| Paragraph ≤ 30 characters | single page |
| Paragraph 30-80 characters, multiple periods | one sentence per page (medium tier font size per page) |
| Paragraph > 80 characters | split roughly every 30 characters, add `⋯` continuation marker |
| List ≤ 4 items | show all on one page (staggered indent) |
| List 5-8 items | split into 2 pages, 3-4 items each (keep item counts close across pages) |
| List > 8 items | split into multiple pages, 4 items each |
| Nested list (e.g. 4 revolutions × 4 attributes) | parent item gets 1 page + each child item becomes its own group (heading page + child-item page) |
| Table ≤ 6 rows | single page |
| Table > 6 rows | split into multiple pages, header row repeated |

**Consistency check**: after splitting, scan through — pages split from the same source should look like the same kind of thing, with font size / indent / background all aligned.

### Auto emphasis (beat markers)

- All `* level-1 headings` → emphasis cover page
- The file's first page (title or first non-empty line of text) → emphasis opening page (merge with the level-1 heading if it already is one)
- The file's last page (last paragraph or last item) → emphasis closing page
- `-----` divider → emphasis pause page
- Everything else → theme pages

Don't force in emphasis pages just to create rhythm — level-1 headings are the natural section breaks.

### Auto hl (highlight)

- org `*emphasis*` → `hl: true`
- org `~code~` `=verbatim=` → `hl: true`
- hl inside emphasis pages is auto-ignored (CSS `color: inherit`)

## Mapping example

**Input** (org excerpt):

```org
#+title: Meituan Talk
#+filetags: :share:

* AI

** Why is AI a revolution?

Human revolution: a tier-shift in ceded capability

- "What it means to be human" redefined
- Social organization reshuffled
```

**Mapping result**:

| # | Type | Content | Source |
|---|---|---|---|
| 1 | emphasis | "AI" | `* AI` (level-1 heading cover) |
| 2 | theme | "Why is AI a revolution?" | `** ...` level-2 heading, own page |
| 3 | theme | "Human revolution: a tier-shift in ceded capability" | paragraph, single sentence |
| 4 | theme | two staggered lines: "'What it means to be human' redefined" / "Social organization reshuffled" | list ≤4 items, one page |

theme auto-selects `red` (filetags `:share:`), title=`Meituan Talk`.

## Visual spec

### Palette (4 colors only)

```
--c-black:  #1A1A1A
--c-red:    #E63956
--c-yellow: #FFD400
--c-white:  #FFFFFF
--c-gold:   #FFE082
```

### Theme mapping (use ≤3 colors per piece)

| theme | default page | emphasis page | hl color (theme pages only) |
|---|---|---|---|
| **black** contemplative | black bg, white text | red bg, white text | red #E63956 |
| **red** manifesto | red bg, white text | black bg, white text | soft gold #FFE082 |
| **yellow** ironic | yellow bg, black text | black bg, white text | red #E63956 |
| **cyber** terminal | black bg, matrix green | green bg, black text | white #FFFFFF (with green glow + CRT scanlines + top HUD) |

### Font stack

```
"Helvetica Neue", "Arial Black", "Inter", "PingFang SC", "Heiti SC", -apple-system, sans-serif
font-weight: 900
letter-spacing: -0.05em
```

Extra font for the cyber theme (used for HUD/footer/pre):

```
"JetBrains Mono", "Fira Code", "IBM Plex Mono", "Source Code Pro", "Menlo", monospace
```

### Adaptive font size

Auto-tiered by character count of the page's "longest line" (CJK characters weighted at 1.8):

| Tier | Character count | Font size |
|---|---|---|
| single | ≤ 2  | 70vmin |
| short  | 3-6 | 48vmin |
| medium | 7-14 | 28vmin |
| long   | 15-26 | 16vmin |
| xlong  | 27+ | 10vmin |

Multi-line pages auto-drop one tier.

### Typesetting

- Content area padding 6vmin 7vmin (close to the edges, so oversized type feels like it fills the frame)
- **lines block horizontally centered + text within lines left-aligned** — `align-items: center` centers the lines block as a whole horizontally on screen (removing 16:9 right-side blank space), but each line of text still starts left-aligned, and indent 0/1/2 creates the stagger within the block
- letter-spacing `-0.05em` — the character-crowding feel proper to ultra-bold
- line-height `1.05`, line gap `0.15em` — multi-line wrapping still has breathing room
- Text vertical direction: centered
- Footer: page number bottom-left, subtitle bottom-right, 13px monospace, opacity 0.5

## JSON Schema

```jsonc
{
  "theme": "black|red|yellow|cyber",      // theme color (required, sets the whole piece's tone)
  "title": "Presentation title (browser tab)",
  "subtitle": "Subtitle/brand (bottom-right footer, optional)",
  "slides": [
    // default theme page
    {
      "lines": [                          // 1-N lines
        {
          "indent": 0,                    // 0/1/2 indent tier (by outline nesting depth)
          "align": "left|center|right",   // optional, default left
          "chunks": [                     // inline segments
            {"t": "leading part of sentence"},
            {"t": "highlighted word", "hl": true},  // only takes effect on theme pages
            {"t": "trailing part of sentence"}
          ]
        }
      ]
    },
    // emphasis page (accent background, the whole page IS the highlight, inline hl not allowed)
    { "emphasis": true, "lines": [...] },
    // pre page (ASCII art / preformatted block)
    { "preTitle": "diagram_name", "pre": "...preformatted text..." }
  ]
}
```

**Field-omission conventions**:
- Omitting `emphasis` = default theme page
- `chunks[].hl: true` inside an emphasis page is ignored
- Writing a `pre` field makes that page an ASCII art page (monospace-rendered)

## Invocation flow

1. **Get the content** (file → Read / pasted → use directly / URL → WebFetch)
2. **Parse the outline**:
   - org: recognize `*` `**` heading hierarchy, `-` `1.` lists, `|...|` tables, `*emphasis*` / `~code~`, `#+begin_example` blocks
   - markdown (compatible): `#` `##` headings, `-` `*` lists, `|` tables, `**emphasis**`, ` ``` ` code blocks
   - plain text (fallback): split into paragraphs by blank lines, one page per paragraph
3. **Infer theme**: explicit argument > `#+filetags:` > default black
4. **Apply mapping rules** to generate the slides array:
   - `*` heading → emphasis cover
   - `**`+ heading → theme page of its own
   - paragraph → theme page (per pagination rules)
   - list → theme page (staggered indent + pagination rules)
   - table → theme page (preserve structure + pagination rules)
   - emphasis markup → auto hl
   - example block → standalone pre page
5. **Read** `assets/slogan_template.html` (the cyber theme needs scanline/HUD/cursor CSS injected on top of the template)
6. **Replace placeholders**:
   - `{{TITLE}}` → file `#+title:` or explicit argument
   - `{{SUBTITLE}}` → `#+author:` `#+date:` concatenated, or left empty
   - `{{THEME}}` → inferred or explicit argument (black|red|yellow|cyber)
   - `{{SLIDES_JSON}}` → JSON.stringify(slides)
7. **Write the file** to `~/Downloads/{name}.html` (`{name}` taken from `#+title:` or the filename, punctuation stripped, ≤ 20 characters)
8. **Report the path** + navigation keys `→ ← Space F Home End`

## Taste guidelines

- **The outline is the truth** — don't change words, don't extract, don't rewrite, don't reorder
- **Level-1 heading = emphasis cover** — a natural section break, sets the beat automatically
- **Level-2 heading = its own theme page** — gives the heading the weight it deserves
- **List staggering** — indent 0/1/2 expresses the outline's nesting depth
- **`*emphasis*` auto-hl** — respect the author's markup intent
- **Consistent splitting** — the same logical block gets the same visual treatment (font tier / indent / background)
- **Keep the footer** — page number + subtitle shouldn't be removed, that's the brand's cool understatement
- **Left-aligned, not centered** — the soul of the VACAT aesthetic

## Off-limits

- **Don't extract a manifesto** — don't go "hunting for the one line," the author has already written the outline
- **Don't write new sentences** — don't reorganize into "complete assertive sentences"
- **Don't reorder** — output in outline order, present it exactly as the author arranged it
- **Don't delete content** — every list item/paragraph must be shown, no cherry-picking
- **No images/icons** — the color blocks ARE the image (except the cyber theme's HUD/scanlines, which are part of that theme)
- **No transition animations** — hard cuts
- **No inline hl on emphasis pages** — the whole emphasis page IS the highlight, adding hl on top would be messy
- **Don't mix multiple themes** — one temperament per piece, no switching
- **Don't make the subtitle too large** — footer is 13px, its presence shouldn't compete with the main title
- **Don't add emphasis on your own initiative** — only level-1 headings, first/last pages, and `-----` are emphasis, nothing else

## Output-language default

Write the output in the same language as the input content. Only fall back to English if the source is in English and the user asks to keep it in English.

## General interaction

- `→` `Space` `Enter` `j` `PageDown`: next page (works with Bluetooth clickers)
- `←` `k` `PageUp`: previous page (works with Bluetooth clickers)
- `Home`/`End`: jump to first/last
- `f`/`F`: toggle fullscreen
- Swipe left/right on touchscreen: page navigation
- Tap right half of screen: next page; tap left half: previous page
