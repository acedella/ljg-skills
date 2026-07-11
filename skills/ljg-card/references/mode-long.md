# Mold: long card (-l / default)

## Step 1: Read the template

Read `assets/long_template.html`

## Step 2: Content preprocessing

- Identify heading lines (starting with `#`/`##`/`###`, or a standalone short line)
- Identify quote blocks (starting with `>`)
- Identify bold text (`**text**`)
- Identify memorable lines: standalone paragraphs, usually < 25 characters, a short sentence that carries as much weight as a full paragraph — render with `.highlight`
- Split into a list of paragraphs on blank lines
- No splitting across cards: all content goes on a single card

## Step 2.5: Tone sensing

Based on the content's mood, choose a background color + accent color pairing so the card matches the content:

| Content mood | `{{BG_COLOR}}` | `{{ACCENT_COLOR}}` | Trigger signal |
|----------|---------------|-------------------|----------|
| Reflective/philosophical | `#FAF8F4` | `#7C6853` | Cognition, thinking, essence, meaning, philosophy |
| Technical/engineering | `#F5F7FA` | `#3D5A80` | Architecture, models, algorithms, systems, code |
| Literary/narrative | `#FBF9F1` | `#6B4E3D` | Stories, characters, writing, prose, poetry |
| Scientific/research | `#F4F8F6` | `#2D6A4F` | Experiments, data, discovery, papers, research |
| Default | `#FAFAF8` | `#4A4A4A` | When nothing else fits |

How to decide: scan the content for high-frequency keywords and themes, pick the closest-matching pair. When unsure, use the default — a wrong-fit pairing is worse than none.

## Step 3: Format as HTML

Basic elements:

- Regular paragraph → `<p>text</p>`
- Section heading (##/### level) → `<h2>heading</h2>`
- Quote → `<blockquote><p>quote</p></blockquote>`
- Bold → `<strong>text</strong>`
- List → `<ul><li>...</li></ul>`

Memorable line (a standalone, incisive short sentence that should visually pop):

```html
<p class="highlight">memorable line text</p>
```

Criteria: standalone paragraph, < 25 characters, states the crux of the matter. Use `.highlight`, not `<p><strong>`.

Emphasis callout (green-background highlight, for key questions/prompts):

```html
<p class="prompt">emphasis callout text</p>
```

Criteria: "prompting" content meant to make the reader pause and look closely. Typical scenarios:

- The Question in a Q&A (the Answer stays a regular paragraph)
- A pointed follow-up question ("So then why isn't X Y?")
- A reading cue ("At this point, stop and think for a moment")
- A sentence directing the reader to go do something

Visually: a pale green background + dark green left border, distinct from `.highlight` (left border + large font size):

- `.highlight` = the author's own memorable conclusion
- `.prompt` = a question/prompt thrown to the reader

No more than 5 `.prompt` blocks per card — too many and the highlighting stops standing out.

Drop cap (first body paragraph):

Add the `dropcap` class to the first regular paragraph (not `.subtitle`, `.highlight`, or `.item`):

```html
<p class="dropcap">first paragraph text...</p>
```

Use only on the first body paragraph — a classic editorial opening style.

Item groups (parallel items with a label + body):

```html
<div class="item">
  <p class="label">Item label</p>
  <p>Item body text</p>
</div>
```

Subtitle tag:

```html
<p class="subtitle">tag text</p>
```

Divider (between sections):

```html
<div class="divider"></div>
```

## Step 4: Render the template

Replace the template variables:

| Variable | Rule |
|------|------|
| `{{BG_COLOR}}` | The background color chosen in step 2.5 |
| `{{ACCENT_COLOR}}` | The accent color chosen in step 2.5 |
| `{{TITLE_BLOCK}}` | If there's a title: `<div class="title-area"><h1>title</h1></div>`; if not, empty string |
| `{{BODY_HTML}}` | All the HTML generated in step 3 |
| `{{SOURCE_LINE}}` | Content source (optional): `<span class="info-source">source text</span>`, empty string if no source |

Write to: `/tmp/ljg_cast_long_{name}.html`

## Step 5: Screenshot

```bash
node assets/capture.js /tmp/ljg_cast_long_{name}.html ~/Downloads/{name}.png 1080 800 fullpage
```
