# Mold: big fonts (-b)

One sentence, slammed onto paper: a milky washi (和紙) background, ink-black stone-inscription (碑刻) style big characters as the body, a seal and signature in the bottom-left. The outer frame is dark, like a black slab propped underneath in the night; the inside carries cloudy bloom, like a water stain not yet dry. The characters are irregular, with chiseled rough edges and water-soaked mottling — that's age, not decoration.

Aim for three qualities:

- "Weight": font size large enough that each line holds 2-5 characters, readable in one glance; every stroke presses down into the paper
- "Age": the paper has cloudy bloom, the characters have rough edges; this aged look is displaced stroke by stroke via SVG turbulence — a Photoshop filter can't produce it
- "Suspension": the card has a dark outer frame, a deep shadow, and a thin inner glow, like a sheet of paper suspended in midair

## Step 1: Read the template

Read `assets/big_template.html`

The template provides:

- A fixed 1080x1440 canvas (Xiaohongshu (小红书) 3:4 HD spec)
- An outer dark background `--bg-dark` that automatically forms a shadow border (22px inset)
- A washi-paper card 1036x1396 + multiple radial-gradient cloud blooms + SVG noise paper grain
- `<filter id="weathered">` feTurbulence + feDisplacementMap for the weathered look (already configured with baseFrequency=0.82, scale=1.8)
- `.main-text` default serif stone-inscription characters (Noto Serif SC 900 + carved-stroke text-shadow)
- `.signature` bottom-left signature block (logo + Li Jigang (李继刚))
- Template variables: `{{FONT_SIZE}}` `{{MAIN_TEXT}}` `{{SOURCE_LINE}}` `{{CUSTOM_CSS}}`

## Step 2: Understand the content

### 2.1 Content shape

`-b` only casts a single sentence/short passage, never a long piece. Typical input:

- A single opinion/judgment (15-30 characters)
- A single rhetorical question (10-20 characters)
- A single memorable line/slogan (8-25 characters)
- At most two sentences (60 characters total, max)

If the input exceeds 60 characters, tell the user: the content is too long, the `-b` mold won't work — suggest switching to `-l` long card or `-m` multi-card.

Note: character counts above assume Chinese-language content. When the content is in another language (e.g. English), apply the same principle by word/character count proportionally, and let the sentence length — not a fixed character count — be the guide for whether `-b` still fits.

### 2.2 Calculating font size

Character count n (for Chinese content: characters, punctuation, and spaces all count, full-width/half-width each count as 1; for other languages, use an equivalent visual-weight estimate):

| Character count | `{{FONT_SIZE}}` | Reference characters per line |
|--------|-----------------|-------------|
| ≤ 10   | 220             | 3-4         |
| 11-16  | 190             | 4-5         |
| 17-24  | 160             | 5-6         |
| 25-34  | 135             | 6-7         |
| 35-46  | 115             | 7-9         |
| 47-60  | 98              | 8-10        |

This is a baseline — it can flex 10-15% up or down depending on how the actual line breaks look.

### 2.3 Manual line breaks

Don't gamble on the browser's automatic wrapping. Insert `<br>` inside `{{MAIN_TEXT}}` and control each line's meaning explicitly.

Line-break principles:

- Cut along semantic units, never in the middle of a word
- Keep line lengths close to each other (difference ≤ 2 characters) for visual stability
- Punctuation stays at the end of a line, never starts a new one
- Put key words/rhetorical-question words at the start of a new line for more impact

Reference line break (for the sentence "In an age where everyone owns a 'Sharingan,' where's the scarcity?", originally in Chinese: "人人拥有「寫輪眼」的时代，稀缺性在哪里？"):

```
人人拥有
「寫輪眼」
的时代，
稀缺性在
哪里？
```

3-4 characters per line, the rhetorical question lands on the last two lines, so all the force lands at the ending.

### 2.4 Highlighting (optional)

To emphasize a word/phrase: wrap it as `<span class="shu">key word</span>` to tint it cinnabar red (朱砂红). At most 1 highlight per image, used on the conclusion sentence or the key point of a rhetorical question.

## Step 3: Write the HTML

### 3.1 Main text

```html
Line 1 text<br>
Line 2 text<br>
<span class="shu">Highlight</span>Line 3<br>
Line 4
```

Keep any mixed traditional/simplified characters exactly as in the source (e.g. keep "寫輪眼" in traditional form if that's how it appears).

### 3.2 Optional: subtitle/small text

Add a small annotation line below the main text (source/author/context) using the `.sub` class:

```html
<div class="main-text">
  The main sentence<br>
  split across lines<br>
  <span class="sub">— So-and-so</span>
</div>
```

`.sub` automatically shrinks to 42% of the main font size, softens in color, and skips the weathering filter (small text needs to stay legible).

### 3.3 Source line (optional)

If there's a source, fill in `{{SOURCE_LINE}}`:

```html
<div class="source-line">From "Such-and-such"</div>
```

Leave it an empty string if there's no source.

### 3.4 Variable substitution summary

| Variable | Content |
|------|------|
| `{{FONT_SIZE}}` | Font size value (no px unit), e.g. `160` |
| `{{MAIN_TEXT}}` | Main text HTML (with `<br>` line breaks) |
| `{{SOURCE_LINE}}` | Source line HTML, or empty |
| `{{CUSTOM_CSS}}` | Extra custom CSS (usually left empty — the template already covers it) |

Write to: `/tmp/ljg_cast_big_{name}.html`

## Step 4: Self-check

- [ ] Does the font size match the character count, 2-10 characters per line?
- [ ] Are the line breaks on semantic units, never mid-word?
- [ ] Does the main text have `filter: url(#weathered)` applied for the aged look?
- [ ] Is the font Noto Serif SC 900 (not Inter, not the default PingFang)?
- [ ] Is the color the ink tone `--ink` (#2C2826), not pure black?
- [ ] Is the washi background color `--paper` (#F4EFE6), with cloud-bloom texture?
- [ ] Does the outer dark background form a 22px shadow border?
- [ ] Is the bottom-left signature block present (logo + Li Jigang (李继刚))?
- [ ] Is the cinnabar-red highlight used ≤ 1 time?
- [ ] Overall: are all three qualities — weight, age, suspension — present?

## Step 5: Screenshot

```bash
node assets/capture.js \
  /tmp/ljg_cast_big_{name}.html \
  ~/Downloads/{name}.png \
  1080 1440
```

Don't use fullpage — `-b` is a fixed 1080x1440 canvas.

## Step 6: Delivery

Report the file path, and in one sentence explain how the line breaks and font size were decided.

## Don'ts

- Don't: accept input over 60 characters (switch to -l or -m instead)
- Don't: rely on default browser line wrapping (manual `<br>` is mandatory)
- Don't: use a font size < 90px (loses the meaning of "big fonts")
- Don't: use more than 1 cinnabar-red highlight (more than one and it stops standing out)
- Don't: remove the weathering filter (loses the stone-inscription feel)
- Don't: change the canvas size (1080x1440 is the Xiaohongshu spec, no deviation)
