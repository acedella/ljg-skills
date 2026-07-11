# Mold: infograph (-i)

Style serves the idea. There's no "default layout": every infograph's visual form grows out of the shape of its content. The template only provides the canvas material (font, color, noise texture, signature) — composition, typesetting, and layout are all designed from scratch based on the content.

## Step 1: Read the template

Read `assets/infograph_template.html`

The template is minimal, providing only:

- Font loading (DM Serif Display + DM Sans + KingHwa_OldSong)
- CSS variables (`--bg`, `--green`, `--pink`, `--yellow`, `--ink`, `--ink-light`, `--white`, `--serif`, `--sans`, `--mono`)
- SVG noise texture (auto-applied)
- `.colophon` signature block
- `{{CUSTOM_CSS}}` and `{{CONTENT_HTML}}` slots

No header, no canvas, no utility classes. All CSS goes in `{{CUSTOM_CSS}}`, all HTML goes in `{{CONTENT_HTML}}`.

## Step 2: Understand the idea

### 2.1 Extract metadata

- Title: ≤ 15 characters
- Subtitle: the core idea in one sentence, ≤ 30 characters
- Source: the content's original source (author, website, etc.), for the right side of the footer (optional)
- REF code: `REF—{domain} / {topic}` (position on the canvas is up to you)

### 2.2 Three dimensions

Once you have the content, first judge three things.

Density, which sets the visual breathing rhythm:

| Density | Core content volume | Visual characteristics |
|------|-----------|---------|
| Sparse | ≤ 50 characters can say it all | One giant element dominates the frame. Whitespace ≥ 60% — the emptier, the more commanding. |
| Medium | 50-200 characters | A structured layout. 2-3 main blocks. Whitespace 30-50%. |
| Dense | 200+ characters | Multiple densely packed blocks. Annotations, grids, layering. Whitespace ≤ 30%. A lab-manual feel. |

Structure, which sets the frame's geometry:

| Structure | Signal | Visual geometry |
|------|------|---------|
| Single point | One core concept | One anchor holds the center of gravity, everything else recedes |
| Contrast | A vs B, old vs new | Split, opposition, two poles |
| Hierarchy | Lower layer supports upper layer | Pyramid, staircase, nesting |
| Process | Sequential order | Vertical waterfall, timeline, pipeline |
| Radial | Core + derivatives | Central radiation, hub-and-spoke |
| Parallel | Multiple parallel concepts | Asymmetric grid (equal division forbidden) |

Emotion, which sets the frame's temperature:

| Emotion | Typography style |
|------|---------|
| Contemplative | Heavy whitespace, serif-dominant, low contrast |
| Sharp | Strong contrast, big text, pink accent pops |
| Warm | Green-dominant, rounded layout, handwritten feel |
| Technical | Mono annotations, grid texture underlay, data-dense |

### 2.3 Output your judgment

```
Density: [sparse/medium/dense]
Structure: [single point/contrast/hierarchy/process/radial/parallel]
Emotion: [contemplative/sharp/warm/technical]
Tone: [contemplative/sharp/warm/technical/scientific/creative/business/default]
Anchor: [what is the single largest element in the frame? where does it sit?]
```

### 2.4 Choosing a tone

Pick the tone that best fits the content's theme. The tone determines three core variables, overridden in `{{CUSTOM_CSS}}` against the template defaults.

| Tone | `--bg` | `--green` (structure color) | `--pink` (accent color) | Trigger signal |
|------|--------|-------------------|-------------------|----------|
| Contemplative | `#F5F2ED` | `#C4B5A0` | `#8B5E3C` | Philosophy, cognition, essence, meaning, existence |
| Sharp | `#EDEDF0` | `#6E6E80` | `#D93025` | Critique, deconstruction, controversy, opposition, debate |
| Warm | `#F7F4EF` | `#C8B898` | `#C17F4E` | Humanities, emotion, life, story, growth |
| Technical | `#F0F3F7` | `#8EAAB8` | `#1A936F` | Architecture, systems, algorithms, code, engineering |
| Scientific | `#F2F6F4` | `#7DAE96` | `#D68C45` | Papers, experiments, data, research, discovery |
| Creative | `#F6F3F2` | `#C0A89C` | `#B8432F` | Art, design, creation, aesthetics, inspiration |
| Business | `#F4F3F0` | `#A8A498` | `#2D6A4F` | Business, finance, markets, investment, strategy |
| Default | `#F2F2F2` | `#B8D8BE` | `#E91E63` | When nothing else fits |

Density adjustment (fine-tune on top of the base color):

- Sparse: the accent color can go up 10-15% in saturation. The frame is empty, so the accent needs presence
- Medium: use the base value
- Dense: drop the structure color 10-15% in saturation, so dense layouts don't feel exhausting

Selection principle:

- Scan the content for high-frequency keywords and themes, pick the best-matching tone
- Emotion and tone can diverge: a contemplative emotion with a technical tone is fine
- When unsure, use the default — a wrong-fit tone is worse than none
- Once a tone is chosen, keep it consistent across the whole image

## Step 3: Design the visuals

### 3.1 Material system

Fonts and ink color (shared across all tones):

| Variable | Value | Use |
|------|------|------|
| `--serif` | DM Serif Display → KingHwa_OldSong | Titles, big text, memorable lines |
| `--sans` | DM Sans → KingHwa_OldSong | Body text, labels |
| `--mono` | SF Mono | Data annotations, REF codes |
| `--ink` | `#2D2926` | Main text color |
| `--ink-light` | `#5C5350` | Secondary text |

Dynamic tone (determined by step 2.4):

| Variable | Role | 90/8/2 |
|------|------|--------|
| `--bg` | Canvas background | 90% neutral surface |
| `--green` | Structure color — blocks, borders, sections | 8% structure |
| `--pink` | Accent — 1-2 precise hits across the whole image | 2% emphasis |

Override method: at the very top of `{{CUSTOM_CSS}}`, write:

```css
:root {
  --bg: #F0F3F7;     /* value chosen in step 2.4 */
  --green: #8EAAB8;
  --pink: #1A936F;
}
```

### 3.2 Design freedom

The following have no default value — they're determined entirely by the content.

Anchor position. The title/core element can be:

- Top-left (traditional)
- Dead center (stone-inscription feel)
- Right side, vertical (East Asian aesthetic)
- Bottom (suspenseful reveal)
- A ghosted background text (like terrain)

Frame division. Can be:

- No division at all, full-frame (sparse density)
- Horizontal split (upper and lower worlds)
- Vertical split (left-right contrast)
- Irregular split (clip-path diagonal cut)
- Grid (lab-manual feel when density = dense)

Text size. Optimized for mobile reading (a 1080px canvas shrinks ~2.8x on a phone):

- Ratio of largest to smallest element ≥ 10:1
- Text can go up to 400px (at that point it's no longer "text," it's "terrain")
- Minimum readable annotation no smaller than 24px (~8.7px on a phone)
- Body text no smaller than 40px (~14.4px on a phone)
- Decorative text (REF codes etc. that don't need close reading) minimum 22px

Color. The 90/8/2 rule (color values from step 2.4):

- 90% neutral (`--bg` + `--white` + `--ink` text)
- 8% structure color (`--green`, one block)
- 2% accent (`--pink`, one precise hit)

### 3.3 Density guidance

#### Sparse (≤ 50 characters)

One element dominates the frame entirely. It could be:

- A single 300-420px character/word
- One memorable line spanning the full width
- One formula, surrounded by silence

Other information (etymology, explanation) sits quietly at 24-28px in a corner or at the bottom, not competing for attention.

Reference composition:

```
+-------------------------+
| ref-code        22px    |
|                         |
|                         |
|      SEATED              |
|      400px serif        |
|                         |
|  subtitle   36px        |
|                         |
|  --- quote ---          |
|  44px serif             |
|                         |
| [colophon]              |
+-------------------------+
```

#### Medium (50-200 characters)

2-3 blocks, with a primary and secondaries. Anchor element 120-180px, body 40-44px, subtitle 34-40px.

Don't arrange blocks evenly. Let one big one take 60%, cram the rest together; or let one full-width band break the rhythm.

Reference composition (for inspiration only, not a fixed template):

```
+-------------------------+
| title 140px              |
| subtitle 36px           |
+-----------+-------------+
|           |             |
| core       | etymology/  |
| explanation| data        |
| 40px       | 32px        |
| 2fr        | 1fr         |
|           |             |
+-----------+-------------+
| ###### dark full-width band ######|
| core formula / one sentence 44px |
+-------------------------+
|                         |
| memorable line 48px serif|
|                         |
| [colophon]              |
+-------------------------+
```

#### Dense (200+ characters)

Dense but ordered frame. Multiple small blocks, visible annotation layers and grid lines, a lab-manual feel.

Dense doesn't mean crowded. Dense means a lot of information, each piece in its place. Use lines, numbering, and color blocks to layer the hierarchy. Body 36-40px, annotations 28-32px, title 80-108px.

Reference composition:

```
+----------+--------------+
| title 84px| ref-code 22px|
| sub 34px | x data  28px |
+----------+--------------+
| +----+ +----+ +------+ |
| | 01 | | 02 | |      | |
| |concept| |concept| |  03  | |
| |36px| |36px| | big concept | |
| +----+ +----+ | 40px | |
|               +------+ |
+-------------------------+
| annotation zone - 28px mono |
| citation - data source - 28px |
| [colophon]              |
+-------------------------+
```

### 3.4 Things to catch yourself doing

If you notice yourself doing any of these, stop:

| If you catch yourself doing this... | Stop |
|------------------------|-------|
| Writing `.header { padding: 56px }` | You're thinking in old-template terms. Start from the content, not from a header. |
| Every block has a white background | At least one should use `--green` or `--ink` |
| Three equal columns | Forbidden. Use `2fr 1fr`, `1fr 340px`, one big two small. |
| Centered title | Unless density=sparse and structure=single point. Otherwise left-align or use an untraditional position. |
| Every image has a "formula band" | It's not required. Some ideas don't have a formula. |
| Accent color used in 3+ places | Pull back to 2. |
| Not a single element over 100px | Find the one worth blowing up. |
| All text sits between 30-44px | The tension isn't stretched enough. Largest and smallest should differ by 10:1. |
| Body text under 36px | Unreadable on mobile. Minimum body 36px, minimum annotation 24px. |
| Equal spacing between every block | Deliberately alternate dense and sparse. |
| Using `max-width` to squeeze text that should fit on one line | Canvas is 1080px. If a sentence fits on one line, let it. Only use `max-width` to control line width (≤ 56ch) for body paragraphs; don't constrain titles and memorable lines — let them stretch out naturally to where they should stop. |

## Step 4: Write the CSS + HTML

Write all CSS into `{{CUSTOM_CSS}}`, all HTML into `{{CONTENT_HTML}}`.

Write the CSS from scratch — don't copy class names or structure from any previous version. Each image's class names should reflect that image's content (`.etymology`, `.core-split`, `.timeline`), not generic names (`.section`, `.panel`, `.label`).

Variable substitution:

| Variable | Content |
|------|------|
| `{{CUSTOM_CSS}}` | All CSS for this image |
| `{{CONTENT_HTML}}` | All HTML for this image |
| `{{SOURCE_LINE}}` | Content source (optional): `<span class="info-source">source text</span>`, empty string if no source |

Write to: `/tmp/ljg_cast_infograph_{name}.html`

## Step 5: Self-check

- [ ] Did this image's visual form grow out of the shape of the content?
- [ ] If you swapped in completely different content, would this layout still make sense? It should — you're making a template, not a design.
- [ ] Ratio of largest to smallest element ≥ 10:1?
- [ ] Accent color used ≤ 2 places?
- [ ] Does the tone match the content's theme? Would this color set still fit if the theme changed?
- [ ] Is there one element that grabs attention at first glance?
- [ ] Is the whitespace intentional, or just leftover?
- [ ] If you told someone "this was made by AI," would they believe it instantly? If yes — redo it.
- [ ] Mobile check: body ≥36px? annotations ≥24px? line height ≥1.6? still comfortable to read after shrinking 2.8x?

## Step 6: Screenshot

```bash
node assets/capture.js /tmp/ljg_cast_infograph_{name}.html ~/Downloads/{name}.png 1080 800 fullpage
```
