---
name: ljg-card
description: "Content caster (铸). Transforms content into PNG visuals. Seven molds: -l (default) long reading card, -i infograph, -m multi-card reading cards (1080x1440), -v editorial sketchnote (problem→failure→pivot→insight→naming, magazine + archive layout), -c comic (manga-style B&W), -w whiteboard (marker-style board layout), -b big-fonts attachment card (1080x1440, weathered stone-inscription (碑刻) style for Xiaohongshu (小红书)). Output to ~/Downloads/. Use when user says '铸' (cast), 'cast', '做成图' (turn into an image), '做成卡片' (make a card), '做成信息图' (turn into an infograph), '做成海报' (turn into a poster), '视觉笔记' (sketchnote), 'sketchnote', '杂志' (magazine), 'editorial', '漫画' (comic), 'comic', 'manga', '白板' (whiteboard), 'whiteboard', '大字' (big fonts), '附件图' (attachment image), 'big fonts', '小红书卡片' (Xiaohongshu card). Replaces ljg-cards and ljg-infograph."
user_invocable: true
version: "3.0.0"
---

# ljg-card: cast (铸)

Content goes in, a PNG comes out. The mold determines the shape.

## Parameters

| Parameter | Mold | Size | Notes |
|------|------|------|------|
| `-l` (default) | Long card | 1080 x auto | Single reading card, content auto-expands the height |
| `-i` | Infograph | 1080 x auto | Layout grows with the content, no fixed template |
| `-m` | Multi-card | 1080 x 1440 | Automatically split into multiple reading cards |
| `-v` | Sketchnote | 1080 x auto | Explains one concept like a magazine feature: problem→failure→pivot→insight→naming |
| `-c` | Comic | 1080 x auto | Japanese-style black-and-white manga, artist chosen to match the content's mood |
| `-w` | Whiteboard | 1080 x auto | Handwritten reasoning chain on a washi-paper background, arrows linking the concepts |
| `-b` | Big fonts | 1080 x 1440 | Large stone-inscription characters + washi paper + drop shadow, for Xiaohongshu (小红书) attachments (single sentence/short paragraph) |

## Constraints

The output is a visual file (PNG); the L0 Org-mode, Denote, and ASCII-only conventions don't apply.

## General rules

### Getting the content

- URL: fetch with WebFetch
- Pasted text: use directly
- File path: read with Read

### File naming

Extract a title or the core idea from the content as `{name}` (use the original language directly, strip punctuation, ≤ 20 characters).

### Screenshot tool

```bash
node assets/capture.js <html> <png> <width> <height> [fullpage]
```

Run from the skill's root directory; depends on playwright in the root-level `node_modules/`. If it errors:

```bash
npm install playwright && npx playwright install chromium
```

### Footer

- Left: logo + Li Jigang (李继刚) (hardcoded in the template)
- Right: content source, optional. If there's a clear source (author name, arxiv ID, site name), fill in `{{SOURCE_LINE}}`: `<span class="info-source">source text</span>`; if not, leave it an empty string. Applies to `-l`, `-i`, `-v`, `-c`, `-w` (`-m` multi-card has no footer).

### Delivery

Report the file path.

## Taste guidelines

No matter which mold you use, Read `references/taste.md` first — it's the visual baseline shared across all molds: no Inter font, no pure black, no three-equal-column cards, no AI-copywriting tone, no fake data.

## Execution

Pick the mold from the parameter, Read `references/taste.md` plus the matching mode file, and follow the steps:

| Parameter | mode file | template |
|------|-----------|------|
| `-l` | `references/mode-long.md` | `assets/long_template.html` |
| `-i` | `references/mode-infograph.md` | `assets/infograph_template.html` |
| `-m` | `references/mode-poster.md` | `assets/poster_template.html` |
| `-v` | `references/mode-sketchnote.md` | `assets/sketchnote_template.html` |
| `-c` | `references/mode-comic.md` | `assets/comic_template.html` |
| `-w` | `references/mode-whiteboard.md` | `assets/whiteboard_template.html` |
| `-b` | `references/mode-big.md` | `assets/big_template.html` |
