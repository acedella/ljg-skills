# Visual spec

Go through this before generating the HTML. Card = light optical-glass card body (spec hardcoded in the template, don't touch) + one embedded AI-generated illustration (the illustration panel).

## Glass card body (Jigang's exact values, don't touch)

| Role | Value |
|------|-----|
| Background gradient | `#eef1f7 → #e6ebf3 → #e3e8f2` cool misty white |
| Glass card surface | `rgba(255,255,255,0.55)` + `backdrop-filter:blur(40px) saturate(1.3)` |
| Highlight edge | `1px solid rgba(255,255,255,0.85)` |
| Soft shadow | `0 8px 32px rgba(31,41,72,0.12)` + `0 40px 80px -32px …` (floating feel) |
| Body text | `#1a1f2e` |
| Secondary | `#5b6472` neutral blue-gray |

Three layers of brightness floating up: misty-white base, glass surface, text. The card body overall stays light-colored; the generated image in the illustration panel comes with its own warm-toned background, the only heavy block of color on the card — the light/warm contrast is intentional design.

## Accent color

`python3 assets/extract_color.py <cover>` extracts the dominant color to fill `{{ACCENT}}`, used in four places: tags, English line, `<span class="hl">` keyword highlights, signature stamp. Change the book, the color changes automatically. For how to handle beige/gray covers and bright covers, and the principle that the card-body color and the illustration-panel palette don't interfere with each other, see the accent-color section in SKILL.md.

## Fonts

| Purpose | Font |
|------|------|
| Viewfinder main sentence / Feynman explanation / book title / subtitle | Noto Serif SC (literary feel) |
| Tags / EN / block-label | JetBrains Mono |
| Author name | Noto Sans SC |

The text in the illustration is handwritten text that comes with the generated scene, not managed by CSS.

## Structure

```
Identity area: cover(198×287) | title+English+subtitle+tags+author avatar
─────
Viewfinder block: label + main sentence {{FRAME}} + Feynman explanation {{EXP}} (justified)
─────
Illustration block: AI-generated illustration {{SKETCH_IMG}} (16:9)
─────
Signature: stamp "Li Jigang"
```

No header, no date, no collection number — Tufte-style, every bit of ink goes to information.

## Illustration panel

`.plate` is a framed container: `border-radius:16px; overflow:hidden`, containing a 16:9 generated image inside (`<img class="sketch">` full width). The image comes with its own background; the panel only adds rounded corners and a soft shadow, CSS hardcoded in the template. For style and protagonist mechanics see SKILL.md; for frame-writing and image verification see extraction.md.

## Adaptivity and alignment

- Card uses `flow + height:auto`, must render with `fullpage`, height follows content, no blank space at the bottom.
- Viewfinder text uses `text-align:justify; text-justify:inter-ideograph`, flush on the right.
- Generated image uses `width:100%`, 16:9 fills naturally.

## Factory self-check

- [ ] Did the cover and avatar load (correct `file://` paths)?
- [ ] Do the main sentence + Feynman explanation thoroughly convey the picture, with keywords colored?
- [ ] Is the card accent color extracted from the cover, harmonious with the cover, only accenting sparingly?
- [ ] Is Jigang recognizable in the generated image? Does the picture read at a glance?
- [ ] Are the Chinese labels correct and not blurry?
- [ ] Do the text and image depict the same picture (is the generated image what `{{FRAME}}`/`{{EXP}}` describe)?
- [ ] Is the text aligned on both ends, no gap on the right?
- [ ] Does the height adapt, no blank space at the bottom?

If any item fails, go back to SKILL.md's Gotchas for the corresponding fix.
