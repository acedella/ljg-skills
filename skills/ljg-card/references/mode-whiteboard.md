# Mold: whiteboard (-w)

Write the reasoning process out so it can be seen: concepts strung together with arrows into a chain, key words called out, simple icon sketches alongside. It advances but doesn't rush — each line takes one step of reasoning, with room to breathe between them. What the board holds onto is how the thinking got there step by step; the conclusion is just the last panel.

Three words set the tone:

- "Negative space" (余白): the space itself is the protagonist. The empty area around the content and between paragraphs is itself the design
- "Austerity" (枯): restrained color, speaking through subtle differences in brightness and warmth. An overall warm-gray tone, with a single dab of cinnabar as the accent
- "Plainness" (素): no decoration. No piled-up borders, no noise texture, no faux-material feel. Clean down to just content and air

## Step 1: Read the template

Read `assets/whiteboard_template.html`

The template provides:

- Handwriting font loading (Permanent Marker + Kalam, mapped to `--marker` / `--hand`)
- CSS variables: `--bg` (outer warm-gray background `#EBE5DA`), `--board` (milky washi board surface `#F7F3EC`), `--ink` (ink), `--ink-light` (light ink gray), `--yellow` (roasted-tea highlight), `--red` (limestone-gray arrow), `--shu` (cinnabar), `--blue` / `--green` / `--orange` (spare colors), `--marker-bg`
- `.board-frame` frame: an extremely faint warm-gray line, almost invisible against the background
- Five-color SVG arrow markers (`arrow-r` / `arrow-w` / `arrow-b` / `arrow-g` / `arrow-y`)
- `.colophon` signature block
- `{{CUSTOM_CSS}}` and `{{CONTENT_HTML}}` slots

## Step 2: Understand the content, choose a style

### 2.1 Extract the structure

Extract from the content:

- Core argument: summarize in one sentence
- Reasoning chain: how was the argument derived step by step? Identify the A → B → C structure
- 3-8 key concepts: candidates for chain nodes
- Branch points: where does the reasoning fork, converge, or turn?
- Drawable concepts: which ones can be quickly expressed as a simple sketch?

### 2.2 Choose a style track

| Style | Visual signature | Trigger signal | Main color |
|------|---------|---------|------|
| Logic chain (default) | Horizontal reasoning chain (connected with →) + vertical hierarchy + roasted-tea keywords + embedded sketches | Content has causal/reasoning/argumentative/explanatory structure | `--red` arrows + `--yellow` highlights |
| Brainstorm wall | Core word centered + radiating branches + colored sticky-note blocks + scattered keywords | Divergent/multi-viewpoint/creative/brainstorming content | `--yellow` dominant |
| Timeline | Vertical timeline + nodes + side notes + contrasting colors | Time/stages/progression/retrospective content | `--green` dominant |
| Matrix analysis | 2x2 or multi-cell matrix + quadrant labels + scattered factors | Classification/comparison/evaluation/decision-framework content | `--blue` dominant |

Selection principle:

- Default to logic chain — it best shows the reasoning process
- Multiple parallel viewpoints → brainstorm wall
- A time/stage dimension → timeline
- Involves classification/quadrants → matrix analysis

### 2.3 Tone: milky washi + cinnabar

Japanese minimalism. A milky background like hand-pulled washi paper, ink-colored text like brushed calligraphy, cinnabar red like the touch of a seal — the finishing accent. Let structure and font weight do the talking; don't shout with color.

| Variable | Value | Role |
|------|------|------|
| `--board` | `#F7F3EC` washi | Milky background, warm hand-pulled washi paper |
| `--ink` | `#2C2826` ink | Body text, a warm near-black like ink |
| `--ink-light` | `#8A8478` light ink gray | Annotations, asides |
| `--yellow` | `#7A6B4E` roasted tea | Highlighted text — reads calm and legible against the milky background |
| `--red` | `#A09888` stone-gray | Arrows: a quiet limestone gray, there only to guide the eye |
| `--shu` | `#C03C28` cinnabar | The finishing accent — genuine cinnabar red, ≤ 3 uses per board |

Frame: an extremely faint warm-gray line at `rgba(180,170,155,0.15)`, nearly invisible against the background.

Principles:

- Build hierarchy on the milky background through font weight and the roasted-tea color
- Ink on washi is the base contrast for the entire board
- Circling marks and underlines uniformly use `--yellow` (roasted tea)
- Arrows use `--red` (limestone gray), quietly guiding without stealing the scene
- `--shu` (cinnabar) is used only for: the conclusion box's border, the circling mark on the core argument, and the seal beside the signature. ≤ 3 uses per board
- Cinnabar is the only high-saturation color — precisely because it's rare, it catches the eye

#### Content-driven temperature shift

Default to keeping the wabi-sabi gray tone. Depending on the content's theme, the background and highlight can shift slightly warmer or cooler; don't introduce a new hue.

Detection: scan the content's keywords and match them to a dominant type. When multiple types overlap, use whichever theme takes up the most space.

| Type | Trigger words | `--board` | `--yellow` | Glow tone |
|------|--------|-----------|------------|---------|
| Default (washi) | No match | `#F7F3EC` | `#7A6B4E` | Warm cinnabar |
| Technical | AI, algorithm, model, code, architecture, system, API, data, engineering, network | `#F3F4F7` | `#5A6878` | Cool blue |
| Humanities | Philosophy, cognition, meaning, ethics, existence, aesthetics, narrative, history, literature, psychology | `#F7F0E5` | `#8A6A3E` | Deep warm |
| Business | Investment, business, growth, market, valuation, financing, strategy, competition, profit, ROI | `#F4F5F0` | `#5A7054` | Neutral green |

Implementation: when a non-default type matches, add variable overrides + a surface glow overlay at the top of `{{CUSTOM_CSS}}`. Don't override anything for the default type.

Technical:
```css
:root { --board: #F3F4F7; --yellow: #5A6878; }
.board > .surface { background: radial-gradient(ellipse at 25% 20%, rgba(120,140,170,0.05) 0%, transparent 50%), radial-gradient(ellipse at 75% 55%, rgba(150,165,190,0.03) 0%, transparent 45%); }
```

Humanities:
```css
:root { --board: #F7F0E5; --yellow: #8A6A3E; }
.board > .surface { background: radial-gradient(ellipse at 25% 20%, rgba(190,160,120,0.05) 0%, transparent 50%), radial-gradient(ellipse at 75% 55%, rgba(210,180,140,0.03) 0%, transparent 45%); }
```

Business:
```css
:root { --board: #F4F5F0; --yellow: #5A7054; }
.board > .surface { background: radial-gradient(ellipse at 25% 20%, rgba(130,160,130,0.05) 0%, transparent 50%), radial-gradient(ellipse at 75% 55%, rgba(160,185,155,0.03) 0%, transparent 45%); }
```

Principles:

- Only override `--board`, `--yellow`, and the surface glow — leave every other variable untouched
- Keep the shift subtle — subtle enough that it's not obvious in a side-by-side comparison, but the overall mood still differs
- `--ink`, `--red`, and `--shu` never change with the content — the touch of cinnabar always stays the one constant color on the board

### 2.4 Title design

The title is the first thing the eye lands on.

Must do:

- The title is a complete judgment/conclusion, not a single word. When the original title is just a concept name, extract the core argument from the content as the main title, and use the concept name as a subtitle
- Mark 1-2 key words in the main title with `--yellow` (roasted tea); the rest stay ink-colored
- Below the main title, close it off with a handwritten wavy underline (SVG wavy path, opacity 0.3)

Title structure:
```html
<div class="board-title">
  <div class="board-title-sub">subtitle or lead-in (smaller, ink-light)</div>
  <h1 class="board-title-main">ink-colored text<span class="y">roasted-tea keyword</span>ink-colored text</h1>
  <svg class="title-line" width="600" height="8">
    <path d="M0,4 Q150,0 300,4 T600,4" stroke="var(--yellow)" fill="none" stroke-width="2" opacity="0.3"/>
  </svg>
</div>
```

Title CSS reference:
```css
.board-title { text-align: center; padding: 56px 48px 16px; }
.board-title-sub { font: 500 34px/1.4 var(--hand); color: var(--ink-light); margin-bottom: 4px; }
.board-title-main { font: 700 64px/1.2 var(--marker); color: var(--ink); margin-bottom: 8px; }
.board-title-main .y { color: var(--yellow); }
.title-line { display: block; margin: 0 auto; }
```

## Step 3: Design the visuals

### 3.1 Board-surface element toolbox

Build all visual elements with CSS + SVG, no external images.

#### Text hierarchy

| Level | Font | Size | Color | Use |
|------|------|------|------|------|
| Main title | `--marker` | 64-80px | `--ink` ink | The big title at the top of the board, keywords embedded in roasted tea |
| Chain text | `--hand` 700 | 34-40px | `--ink` ink | Concepts and explanations in the logic chain |
| Keywords | `--hand` 700 | 34-42px | `--yellow` | Concepts to emphasize within the chain |
| Annotation | `--hand` 400 | 24-28px | `--ink-light` | Side notes, supplements, small text |
| Large numerals | `--marker` | 72-120px | `--yellow` or `--red` | Data highlights |

Note on non-Latin/CJK text: Permanent Marker and Kalam don't render Chinese characters, and fall back to PingFang SC. When the content is Chinese, the handwritten feel is carried instead through color (roasted tea/cinnabar) and decoration (underlines, circling marks).

#### Handwritten-mark effects (CSS)

```css
/* Chain arrow — the visual cue for logical progression */
.chain-arrow {
  color: var(--red);
  font: 700 34px var(--hand);
  margin: 0 6px;
  display: inline;
}

/* Roasted-tea highlight */
.chalk-yellow {
  color: var(--yellow);
  font-weight: 700;
}

/* Handwritten circle mark */
.chalk-circled {
  border: 2.5px solid var(--yellow);
  border-radius: 45% 55% 50% 48%;
  padding: 2px 14px;
  display: inline-block;
}

/* Underline */
.chalk-underline {
  border-bottom: 3px solid var(--red);
  padding-bottom: 2px;
}

/* Box (for an important conclusion) */
.chalk-box {
  border: 2.5px solid var(--ink);
  border-radius: 3px;
  padding: 14px 18px;
}

.chalk-box-red { border-color: var(--red); }
.chalk-box-yellow { border-color: var(--yellow); }
.chalk-box-shu { border-color: var(--shu); }

/* Dashed box */
.chalk-dashed {
  border: 2px dashed var(--ink-light);
  border-radius: 3px;
  padding: 14px 18px;
}

/* Numbered marker */
.chalk-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px; height: 36px;
  border-radius: 50%;
  border: 2.5px solid var(--red);
  color: var(--red);
  font: 700 22px var(--hand);
  margin-right: 8px;
}

/* Question mark / exclamation mark badge */
.chalk-question {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px; height: 44px;
  border: 2.5px solid var(--yellow);
  border-radius: 50%;
  color: var(--yellow);
  font: 700 28px var(--hand);
}

/* Cinnabar accent — used extremely sparingly, ≤ 3 per board */
.shu-circle {
  border: 2.5px solid var(--shu);
  border-radius: 45% 55% 50% 48%;
  padding: 2px 14px;
  display: inline-block;
}

/* Cinnabar seal — a square seal beside the conclusion, the wabi-sabi finishing touch */
.shu-stamp {
  display: inline-block;
  border: 2px solid var(--shu);
  padding: 4px 10px;
  font: 700 18px var(--hand);
  color: var(--shu);
  transform: rotate(-3deg);
  opacity: 0.7;
}
```

#### Simple sketches (inline SVG)

Use simple SVG paths to draw concept icons. Use `var(--ink)` or `var(--yellow)` for stroke, stroke-width: 2-3px, stroke only, no fill (a handwritten line-art style). 3-5 paths per icon.

Common ones: a person (round head + line body + limbs), a question mark, a trend chart (a folded line), a house (triangle + square), an animal (a minimal outline), a light bulb, a lightning bolt.

Handwritten feel: lines shouldn't be perfectly straight — a path can carry a slight wobble.

### 3.2 Layout principles

The board surface is chain-like: the main body is a horizontal logic chain, progressing in vertical layers.

Core rules:

- Chain flow: each line is one reasoning chain, concepts connected with → from left to right
- Vertical hierarchy: each layer is the next step of the reasoning, related to the layer above/below through indentation or spacing
- Embedded icons: sketches and text mix inline on the same line, not given their own separate zone
- Roasted-tea highlights: important concepts use `--yellow`, visible at a glance against the ink-colored text
- Arrows uniformly `--red` (limestone gray), the visual beat of logical progression
- Vary density: keep the core reasoning chain tight (1.8x line spacing), and leave breathing room (間, ma) at topic transitions
- Whitespace is content: don't aim to fill the board. Leave large gaps before and after a major turn, giving the reader's thinking somewhere to land
- Natural misalignment: the chain doesn't need to be strictly left-aligned — let it drift the way real handwriting naturally does

#### Spacing hierarchy (for separating zones)

Let spacing do the talking instead of lines. Don't draw dividing lines on the board — only tight writing and loose writing.

| Tier | Spacing | Use | CSS |
|------|------|------|-----|
| Zero gap | 0-4px | A continuation line of the same chain | `.cl + .cl { margin-top: 2px; }` |
| Small gap | 14-20px | Different chains within the same topic | `margin-top: 16px` |
| Medium gap | 32-44px | Between different topics (間) | `margin-top: 36px` |
| Large gap | 52-72px | A major turn/new chapter (大間) | `margin-top: 60px`, can add a handwritten wavy line |

Forbidden:

- `border-top` straight-line dividers — no straight lines to divide zones on the board
- Overusing ↓ arrows — only use them for an explicit "therefore/so" progression, ≤ 3 per board
- Evenly-spaced layout — uneven spacing is what makes it feel handwritten

Handwritten wavy line (for large-gap separation only, optional):
```html
<svg width="800" height="8" style="display:block;margin:0 auto;opacity:0.15;">
  <path d="M0,4 Q100,0 200,4 T400,4 T600,4 T800,4" stroke="var(--ink)" fill="none" stroke-width="1.5"/>
</svg>
```

### 3.3 Visual composition (by style)

#### Logic chain (default)
```
[Big title — ink color, centered, keyword embedded in roasted tea]

→ Concept A → Observe X → Rule out Y → Derive Z → Conclusion 1
                                                  |
                                                  v
→ Expand conclusion 1 → [sketch] → Additional note → Derive concept B
                                                  |
                                                  v
    Concept B → For example... → But there's a problem → [?]
                                                  |
                                                  v
→ Answer... → Therefore → [Final conclusion — cinnabar box]
```
- Each line is one reasoning chain, lines connected with a ↓
- Lines starting with → are the main thread of reasoning, those without are supplementary
- Let the chain flow naturally, as if writing left-to-right on the board
- Sprinkle in sketches where appropriate, to break up the monotony of pure text

#### Brainstorm wall
```
       [color block 1]   [color block 2]
            \     /
     [block 3] → [core word] ← [block 4]
            /     \
       [block 5]   [block 6]

  Bottom: key conclusion, underlined
```

#### Timeline
```
  [Title]

  o------o------o------o------o
  Stage1  Stage2  Stage3  Stage4  Stage5
  |       |       |       |       |
  Note    Note    Note    Note    Note
```

#### Matrix analysis
```
  [Title]
            | Dimension A high
    --------+--------
    Quad 1  | Quad 2
            |
    --------+--------
    Quad 3  | Quad 4
            | Dimension A low
   Dim B low     Dim B high
```

## Step 4: Write the CSS + HTML

Write all CSS into `{{CUSTOM_CSS}}`, all HTML into `{{CONTENT_HTML}}`.

Write the CSS from scratch, with class names reflecting the content's semantics (`.premise-chain`, `.conclusion-box`), not generic names.

Tone variables: for the default wabi-sabi tone, use the template values directly. When the content matches the technical/humanities/business type (see "Content-driven temperature shift"), override `--board`, `--yellow`, and `.board > .surface` at the top of CUSTOM_CSS.

Logic-chain layout tips:

- Each chain uses `display: flex; align-items: baseline; flex-wrap: wrap; gap: 4px 6px;` for natural wrapping
- Arrows use `<span class="chain-arrow">→</span>` inline in the text flow
- Roasted-tea keywords use `<span class="chalk-yellow">keyword</span>`
- Sketch SVGs use `display: inline-block; vertical-align: middle;` to sit inline
- Uneven line spacing — lines within the same point stay tight (`margin-top: 12px`), a new topic gets a larger gap (`margin-top: 28px`)
- Lines starting with → can add `padding-left` indentation, to build up layers

Variable substitution:

| Variable | Content |
|------|------|
| `{{CUSTOM_CSS}}` | All custom CSS |
| `{{CONTENT_HTML}}` | All content HTML |
| `{{SOURCE_LINE}}` | Content source (optional): `<span class="info-source">source text</span>`, empty string if no source |

Write to: `/tmp/ljg_cast_whiteboard_{name}.html`

## Step 5: Self-check

- [ ] Does the washi background + ink-colored text feel comfortable, not harsh?
- [ ] When the content matched the technical/humanities/business type, was the temperature shift (--board, --yellow, glow) applied?
- [ ] Gray tones dominate, cinnabar (--shu) used ≤ 3 times?
- [ ] Is the title a complete judgment sentence? Highlighted words in roasted tea (--yellow)?
- [ ] Is the title big and bold enough (≥ 64px)?
- [ ] Enough negative space — margins ≥ 72px around the edges, paragraph gaps feel like they breathe?
- [ ] Chain text ≥ 34px? Annotations ≥ 24px?
- [ ] Zone separation uses the spacing hierarchy, no border-top straight lines?
- [ ] At least 2 simple icon sketches (SVG), lines in the ink color?
- [ ] No faux-material decoration (no noise texture, no heavy frame, no texture filters)?
- [ ] Overall clean, quiet, and warm?

## Step 6: Screenshot

```bash
node assets/capture.js /tmp/ljg_cast_whiteboard_{name}.html ~/Downloads/{name}.png 1080 800 fullpage
```
