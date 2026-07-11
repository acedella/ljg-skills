# Mold: comic (-c)

Tell the story in the visual language of Japanese black-and-white manga. Use panel rhythm, black-and-white contrast, focus lines, and negative space to create drama — this is more than just slapping a comic-style border on a layout. Color is almost never used, at most one gray screentone. All the punch comes from ink density and how tight or loose the composition is.

## Step 1: Read the template

Read `assets/comic_template.html`

The template provides:

- Font loading (Noto Serif SC + DM Sans)
- CSS variables (`--bg`, `--ink`, `--ink-mid`, `--ink-light`, `--white`, `--accent`, `--tone`)
- SVG filters: `#inkgrain` (ink texture), `#halftone` (screentone), `#roughen` (rough edges)
- `.colophon` signature block
- `{{CUSTOM_CSS}}` and `{{CONTENT_HTML}}` slots

## Step 2: Understand the content, choose a style

### 2.1 Extract narrative elements

Extract from the content:

- Core conflict/tension: what's fighting what in this piece?
- 3-5 key moments: scenes or concepts that can become "panels"
- Emotional arc: from what state to what state?
- Visual anchor: the single most vivid, image-rich concept

### 2.2 Determine card count and narrative arc

Default to a set of 4 cards, running one complete narrative arc: setup → the real culprit/twist → exposure/solution → close. Even short pieces run the full 4-card arc — don't shrink the arc. For very long pieces (> 3000 words / 6+ sections), it can extend to 5 cards to preserve completeness — better one extra card than losing the core argument. Only cast a single card when the user explicitly asks for just one.

Principles:

- Each card stands on its own: its own title, its own panel layout, its own narrative paragraph — don't just hard-chop a long piece to pad out the count
- Series numbering: top-right corner marked `01/N` through `N/N`
- The first card sets up, the last card closes; the last card carries a small `終` (The End) mark in the corner (black background, white text — keep it understated; if it lands on a black panel, flip it to white background, black text, to avoid it disappearing)
- Digest/news-summary content also runs the 4-card arc: pick one paradox as the throughline skeleton and filter out the noise rather than trying to cover everything

### 2.3 Extract images from the source

If the source content has images (a WebFetch markdown result with `![](url)`, or HTML `<img>`):

- Collect all image URLs
- Judge which ones are relevant to the core argument (ignore logos, ads, decorative images)
- Embed relevant images into panels in step 4

### 2.4 Choose a manga style

| Style | Visual signature | Trigger signal | CSS variable override |
|------|---------|---------|-------------|
| Otomo Katsuhiro — precision ruins | Extremely fine, densely packed lines, mechanical/architectural detail, rich grayscale, precise perspective | Technical/systems/architecture/complex mechanisms, high information density | `--tone: #D0D0D0` |
| Inoue Takehiko — ink-wash negative space | Large areas of whitespace, ink-density gradients, visible brushwork, minimalist composition | Philosophical/contemplative/aesthetic/humanistic, needs room to breathe | `--tone: #E8E0D8` |
| Miura Kentaro — dark oppression | Large areas of solid black, extreme contrast, dense texture, heavy oppressive feel | Conflict/hardship/dark side/struggle, intense emotion | `--bg: #F0F0F0; --tone: #C0C0C0` |
| Matsumoto Taiyo — raw thick lines | Lines of uneven thickness, irregular composition, kinetic energy, looks rough but is actually precise | Movement/energy/creativity/breakthrough, has impact | `--tone: #E0E0E0` |
| Taniguchi Jiro — quiet precision | Architectural-grade fine lines, restrained expressions, silver-gelatin-photo-like grayscale, quiet | Everyday life/observation/detail/quiet strength | `--tone: #E5E5E5` |

Selection principle:

- Choose based on the content's mood, referencing the trigger signals above; when the mood is ambiguous, default to Inoue Takehiko (the most versatile black-and-white aesthetic)
- The most literal match isn't always the best fit — a mismatched mood can sometimes land better (dense technical/mechanistic content paired with Taniguchi's quiet restraint can hit harder than the "obvious" choice of Otomo)
- Before settling on a style, check which styles were used in the last few casts (recent output, session memory) and avoid whichever was used immediately prior, so adjacent pieces contrast as much as possible; once all five have been cycled through, reuse is allowed, but still avoid the most recent
- When casting multiple pieces concurrently, assign styles up front before work begins rather than letting each agent choose on its own, to avoid collisions
- Series consistency: for multiple cards from the same article or book, read the whole thing through once and settle on one style to use start to finish. Switching styles concept-by-concept breaks the reading flow

## Step 3: Design the visuals

### 3.1 Manga visual element toolbox

Build all elements with CSS + SVG. Black and white dominate, gray screentone is secondary, no color.

#### Panel layout system

The core of manga is paneling. Use CSS Grid to build irregular panels:

```css
/* Basic panel */
.panel {
  border: 3px solid var(--ink);
  background: var(--white);
  position: relative;
  overflow: hidden;
  padding: 28px 32px;
}

/* Bleed panel (content breaks through the border) */
.panel-bleed {
  border: none;
  margin: -3px;
  z-index: 2;
}

/* Slanted panel */
.panel-slanted {
  clip-path: polygon(0 0, 100% 8%, 100% 100%, 0 92%);
}
```

#### Focus lines

Push a key concept right up to the reader's eye:

```css
.focus-lines {
  position: relative;
}
.focus-lines::before {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-conic-gradient(
    var(--ink) 0deg 0.5deg,
    transparent 0.5deg 5deg
  ) center/100% 100%;
  opacity: 0.06;
  pointer-events: none;
}
```

#### Speed lines

For expressing motion, change, or impact:

```css
.speed-lines {
  background-image: repeating-linear-gradient(
    90deg,
    transparent,
    transparent 4px,
    var(--ink) 4px,
    var(--ink) 4.5px
  );
  opacity: 0.08;
}
```

#### Speech bubbles / thought bubbles

```css
.speech-bubble {
  background: var(--white);
  border: 2.5px solid var(--ink);
  border-radius: 20px;
  padding: 16px 22px;
  position: relative;
  font: 700 32px/1.4 var(--serif);
}
.speech-bubble::after {
  content: '';
  position: absolute;
  bottom: -16px;
  left: 40px;
  border: 8px solid transparent;
  border-top-color: var(--ink);
}

/* Thought bubble (round tail) */
.thought-bubble {
  border-radius: 50% 50% 50% 50% / 40% 40% 60% 60%;
}
.thought-bubble::after {
  width: 12px; height: 12px;
  border-radius: 50%;
  background: var(--ink);
  border: none;
  bottom: -20px;
}

/* Shout bubble (jagged edge) */
.shout-bubble {
  clip-path: polygon(
    0% 20%, 5% 0%, 15% 15%, 25% 0%, 35% 10%,
    50% 0%, 65% 10%, 75% 0%, 85% 15%, 95% 0%,
    100% 20%, 95% 35%, 100% 50%, 95% 65%, 100% 80%,
    95% 100%, 85% 85%, 75% 100%, 65% 90%, 50% 100%,
    35% 90%, 25% 100%, 15% 85%, 5% 100%, 0% 80%,
    5% 65%, 0% 50%, 5% 35%
  );
  background: var(--white);
  padding: 28px 36px;
  font: 900 38px/1.3 var(--serif);
}
```

#### Screentone

Gray in manga isn't gray, it's a dot pattern:

```css
.screentone {
  background-image: radial-gradient(circle, var(--ink) 1px, transparent 1px);
  background-size: 5px 5px;
  opacity: 0.15;
}

/* Fading screentone */
.screentone-gradient {
  background-image: radial-gradient(circle, var(--ink) 1px, transparent 1px);
  background-size: 5px 5px;
  -webkit-mask-image: linear-gradient(to bottom, black, transparent);
  mask-image: linear-gradient(to bottom, black, transparent);
  opacity: 0.2;
}
```

#### Onomatopoeia / sound effects

Big, tilted sound-effect lettering — the soul of manga:

```css
.sfx {
  font: 900 80px/1 var(--serif);
  color: var(--ink);
  transform: rotate(-8deg) skewX(-5deg);
  letter-spacing: -3px;
  text-shadow: 3px 3px 0 var(--tone);
  -webkit-text-stroke: 1px var(--ink);
}
```

#### Ink wash

The core of the Inoue Takehiko line — simulate ink wash with a CSS gradient:

```css
.ink-wash {
  background: linear-gradient(
    135deg,
    var(--ink) 0%,
    rgba(26,26,26,0.6) 20%,
    rgba(26,26,26,0.15) 50%,
    transparent 70%
  );
}
```

### 3.2 Typography principles

Manga has its own set of typesetting rules:

- Vertical text is optional: Japanese manga was originally set vertically. Key text can use `writing-mode: vertical-rl` to create a manga feel
- Extreme size contrast: title 120px+, body 32px, side notes 20px, ratio ≥ 6:1
- Bold = emphasis: in manga, bold means "the voice got louder"
- Whitespace = silence: large blank areas are deliberate pauses, not unfinished drawing
- Black blocks = pressure: pure black regions create oppression and drama
- Pre-break long text: for narrow panels, speech bubbles, and memorable lines, manually insert `<br>` to pre-wrap the text when writing HTML — don't gamble on browser auto-wrap. Before writing, calculate the max characters per line from (column inner width − padding − border) ÷ font size, and write narrow-column copy to that limit; full-width long sentences are more stable left to wrap naturally

### 3.3 Panel composition (by style)

#### Otomo Katsuhiro — precision ruins
```
+------------------+--------+
|                  | detail |
|  wide establishing|  panel |
|  panel (tech      +--------+
|  architecture)    | data   |
+--------+---------+  panel  |
| text   | text    |        |
| panel  | panel   |        |
+--------+---------+--------+
```
Signature: squared panels, precise linework, high information density

#### Inoue Takehiko — ink-wash negative space
```
+-------------------------+
|                         |
|   large area of         |
|      whitespace         |
|      core concept       |
|   (ink-gradient bg)     |
|                         |
+------------+------------+
|  narrow    |   narrow    |
|  panel     |   panel     |
|  point A   |   point B   |
+------------+------------+
```
Signature: heavy whitespace, few panels, each panel carries little but heavy information

#### Miura Kentaro — dark oppression
```
+--+--------------------+--+
|bl|                    |bl|
|ck|  core conflict      |ck|
|  |  wide panel         |  |
|bo|  (heavy black +     |bo|
|rd|   white text)       |rd|
|er|                    |er|
+--+---------+----------+--+
|   dense    |   dense    |
|   panel    |   panel    |
|  (gray     |  (solid    |
|  screentone)|  b/w)     |
+------------+-----------+
```
Signature: large areas of solid black, white-on-black inversion, oppressive feel

#### Matsumoto Taiyo — raw thick lines
```
  +-------+
  | tilted \
 /  panel   +--------+
+   core concept      |
|   (big bold text)   |
+-----+       +------+
      | irregular|
      |  small    |
      |  panel    |
      +---------+
```
Signature: tilted panels, uneven border thickness, full of kinetic energy

#### Taniguchi Jiro — quiet precision
```
+-------------------------+
|   finely-drawn scene     |
|   panel (wide letterbox, |
|    cinematic)            |
+------------+------------+
|            |            |
| square     | square     |
| panel      | panel      |
| detail A   | detail B   |
|            |            |
+------------+------------+
|   bottom narrative panel |
+-------------------------+
```
Signature: regular panels, wide letterbox strips, quiet and even

### 3.4 Handling images from the source

When step 2.3 collected relevant images:

- Prefer the original images: reference the original image URL directly with `<img>`, placed into a manga panel
- Treat the image as a panel: give the original image its own panel, add a 3px border, integrate it into the panel system
- Keep it black-and-white in tone: `filter: grayscale(100%) contrast(1.2);` preserves the manga's black-and-white feel. If the original image is already black-and-white/line-art, its original color may be kept
- Image CSS: `width: 100%; height: auto; object-fit: cover; display: block;`
- If an image is unavailable (404 / CORS), skip it — don't substitute a placeholder image

## Step 4: Write the CSS + HTML

Write all CSS into `{{CUSTOM_CSS}}`, all HTML into `{{CONTENT_HTML}}`.

Write CSS from scratch, with class names reflecting the content, not generic names.

Core constraints:

- Palette limited to three values — black (`--ink`), white (`--white/--bg`), gray (`--tone`) — with occasional use of `--ink-mid`
- For emphasis, use a solid black-on-white inversion (white text on black), not color
- Borders uniformly 2.5-3px, to simulate a pen stroke
- At least one panel should "bleed" (content breaks through the normal margin) — this is where the manga feel comes from

Variable substitution:

| Variable | Content |
|------|------|
| `{{CUSTOM_CSS}}` | All CSS (including `:root` overrides) |
| `{{CONTENT_HTML}}` | All HTML |
| `{{SOURCE_LINE}}` | Content source (optional): `<span class="info-source">source text</span>`, empty string if no source |

Write to:

- Single card: `/tmp/ljg_cast_comic_{name}.html`
- Multiple cards: `/tmp/ljg_cast_comic_{name}_{N}.html` (N = 01, 02, ...)

## Step 5: Self-check and acceptance

After generating the HTML:

- [ ] Does it look like a manga page at a glance? If it looks like "regular layout with a border added," redo it
- [ ] At least 3 panels? Manga without panels isn't manga
- [ ] Strong black-and-white contrast? Areas of solid black present?
- [ ] At least 1 manga-specific element (focus lines / speed lines / speech bubble / sound effect / screentone)?
- [ ] Contrast in panel size? (one big panel + several small ones, not all equal size)
- [ ] Avoided color? Gray only used as screentone `--tone`
- [ ] Body text ≥ 32px? Titles ≥ 72px?
- [ ] Is there one panel that grabs the eye first?
- [ ] Avoided equal division, symmetry, and even spacing?
- [ ] Is the `終` (The End) mark on the last card present, and not invisible against a black background?

After the screenshot, verify each card:

- Open the PNG and check with your own eyes: is the panel structure correct, do the SVG images make sense (draw a "compass" not something that looks like a clock), any text overflow?
- If you suspect text errors in the rendered image, always grep the HTML source to verify — don't judge from OCR-reading the rendered image; small text is easy to misread. Use the rendered image only to check panel layout / image meaning / overflow / grayscale
- After any edit and re-render, confirm the PNG mtime ≥ the HTML mtime, to avoid delivering a stale image
- rg-scan the HTML for all color declarations: everything should be grayscale or a uniform near-neutral paper tone, no color hues, no `#000000`

## Step 6: Screenshot

Single card:
```bash
node assets/capture.js /tmp/ljg_cast_comic_{name}.html ~/Downloads/{name}.png 1080 800 fullpage
```

Multiple cards: screenshot each one, filenames carry the sequence number
```bash
node assets/capture.js /tmp/ljg_cast_comic_{name}_01.html ~/Downloads/{name}_01.png 1080 800 fullpage
node assets/capture.js /tmp/ljg_cast_comic_{name}_02.html ~/Downloads/{name}_02.png 1080 800 fullpage
# ... one per card
```

## Step 7: Delivery

Report the file paths, the card count, the chosen style, and the reasoning. Append one closing line, a "reflection": connect this piece to one axis of the four-axis worldview (evolution / game theory / cybernetics / neuroscience), landing the isomorphism in a single sentence. Avoid whichever axis was used most recently; if it doesn't fit naturally, don't force it.
