# Mold: multi-card (-m)

## Step 1: Read the template

Read `assets/poster_template.html`

## Step 1.5: Tone sensing

Shares the same tone system as the long-card mold. Based on the content's mood, choose `{{BG_COLOR}}` and `{{ACCENT_COLOR}}`:

| Content mood | `{{BG_COLOR}}` | `{{ACCENT_COLOR}}` | Trigger signal |
|----------|---------------|-------------------|----------|
| Reflective/philosophical | `#FAF8F4` | `#7C6853` | Cognition, thinking, essence, meaning, philosophy |
| Technical/engineering | `#F5F7FA` | `#3D5A80` | Architecture, models, algorithms, systems, code |
| Literary/narrative | `#FBF9F1` | `#6B4E3D` | Stories, characters, writing, prose, poetry |
| Scientific/research | `#F4F8F6` | `#2D6A4F` | Experiments, data, discovery, papers, research |
| Default | `#FAFAF8` | `#4A4A4A` | When nothing else fits |

## Step 2: Content preprocessing

- Identify heading lines (starting with `#`/`##`/`###`, or a standalone short line)
- Identify quote blocks (starting with `>`)
- Identify bold text (`**text**`)
- Identify memorable lines: standalone paragraphs, usually < 25 characters, a short sentence that carries as much weight as a full paragraph — render with `.highlight`
- Split into a list of paragraphs on blank lines

## Step 3: Calculate visual weight

The template renders at full 1080x1440 resolution, body text 36px, line height 1.7.

- Regular paragraph: character count × 1.4
- Heading line (h1 on the first card, 84px): character count × 6.0
- Memorable line (`.highlight` 40px + left border + top/bottom whitespace): character count × 3.0
- `.item` group (label + body): character count × 1.8
- Quote block: character count × 1.7
- Divider: fixed weight of 60
- Code block: character count × 2.2
- Running title (continuation-page header): fixed weight of 70

## Step 4: Greedy splitting

- Threshold: about 380 characters of equivalent visual weight per card
- Accumulate paragraph by paragraph; cut before the current paragraph once the threshold is exceeded
- Splitting rules:
  - Never cut in the middle of a sentence
  - Prefer cutting at paragraph/item/section boundaries
  - A heading never stands alone (must share a card with at least one content element)
  - An overly long single paragraph is force-cut at a sentence boundary
  - One section (h2 + 3 items) usually fits exactly on one card

Special cases:

- Only one card: no page number shown
- Multiple cards: show a `1 / N` style page number

## Step 5: Format as HTML

Basic elements:

- Regular paragraph → `<p>text</p>`
- Section heading (##/### level) → `<h2>heading</h2>`
- Quote → `<blockquote><p>quote</p></blockquote>`
- Bold → `<strong>text</strong>`
- List → `<ul><li>...</li></ul>`

Memorable line (a standalone, punchline-like short sentence that should visually pop):

```html
<p class="highlight">memorable line text</p>
```

Criteria: standalone paragraph, < 25 characters, carries as much weight as a full paragraph. Use `.highlight`, not `<p><strong>`.

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

## Step 6: Render the template

For each card, replace the template variables:

| Variable | Rule |
|------|------|
| `{{BG_COLOR}}` | The background color chosen in step 1.5 |
| `{{ACCENT_COLOR}}` | The accent color chosen in step 1.5 |
| `{{HEADER_BLOCK}}` | Continuation card: `<div class="header"><span class="running-title">article title</span></div>`; first card or single card: empty string |
| `{{TITLE_BLOCK}}` | First card with a title: `<div class="title-area"><h1>title</h1></div>`; continuation card or no title: empty string |
| `{{BODY_HTML}}` | The HTML generated in step 5 |
| `{{SOURCE_LINE}}` | Content source (optional): `<span class="info-source">source text</span>`, empty string if no source |
| `{{PAGE_INFO}}` | `1 / 3` for multiple cards, empty string for a single card |

Closing mark: append `<p style="text-align:right;font-size:16px;color:#ACACB0;margin-top:40px;">∎</p>` only at the end of `{{BODY_HTML}}` on the last card. Don't add it on non-final pages.

Write to: `/tmp/ljg_cast_poster_{name}_{N}.html`

## Step 7: Screenshot

```bash
node assets/capture.js /tmp/ljg_cast_poster_{name}_{N}.html ~/Downloads/{name}_{N}.png 1080 1440
```

Multiple cards can be screenshotted in parallel.

When delivering, report the card count + a summary of each (first 30 characters).
