# Design Taste Guidelines (shared across all molds)

Before any mold generates HTML, run through this checklist. This is the baseline for visual quality.

## 1. Baseline parameters

| Dimension | Default | Meaning |
|------|--------|------|
| DESIGN_VARIANCE | 8 | 1 = perfect symmetry, 10 = artistic chaos |
| VISUAL_DENSITY | 4 | 1 = gallery whitespace, 10 = cockpit-level information density |

These two dials govern three molds:

- `-l` long card: DESIGN_VARIANCE=5, VISUAL_DENSITY=3, reading comfort first. Variation lands on tone: different content moods get different background and accent colors (see mode-long.md step 2.5)
- `-i` infograph: DESIGN_VARIANCE=7, VISUAL_DENSITY=8, data density first. Variation comes from dynamic REF coding and content-driven custom layouts
- `-m` multi-card: DESIGN_VARIANCE=9, VISUAL_DENSITY=2, visual impact first. Shares the tone system with the long card; the ∎ closing mark only appears on the final page

The other four molds (`-v` sketchnote, `-c` comic, `-w` whiteboard, `-b` big fonts) each have their own aesthetic system, written up in their own mode files — these two dials don't apply to them. The rest of this document is the shared baseline across all seven molds; where a mode file explicitly states its own rule (e.g. `-v`'s four-main-color system, `-b`'s centered stone-inscription layout), the mode file takes precedence.

## 2. Typography engineering

### Headings
- Large headings: `tracking-tighter` (tight letter spacing), `leading-none` (minimal line height)
- No Inter font. Long card/multi-card use a serif (Noto Serif SC); infograph mixes monospace + sans-serif
- Dashboard/technical scenes must never use serif — only premium sans-serif (Geist, Satoshi, Cabinet Grotesk)

### Body text
- Default: `text-base`, `leading-relaxed`, max line width `65ch`
- `-i` infograph: body ≥36px, line height ≥1.6, annotations ≥24px (a 1080px canvas shrinks ~2.8x to a 390px phone screen, and must still be readable after shrinking)
- Avoid pure black for paragraph text; use a deep gray like `#333` or `#4a4a4a`

### Numbers
- When VISUAL_DENSITY > 7 (infograph), render all numbers in a monospace font (`font-family: monospace`)

## 3. Color calibration

### Hard rules
- At most 1 accent color, saturation < 80%
- No "AI purple-blue": no purple button glow, no neon gradients, ever
- Keep warm/cool tone consistent within a single image; don't waver between warm gray and cool gray
- No pure black `#000000`: use Off-Black (`#1a1a1a`), Zinc-950, or charcoal gray

### Gradient constraints
- Don't fill large headline text with a gradient
- Background gradients should only be subtle transitions, never abrupt color jumps

## 4. Layout diversity

### When DESIGN_VARIANCE > 4
- No centered hero: don't default headlines to center. Use left alignment, split screens, asymmetric whitespace
- No "three equal-width cards": three equal columns side by side is the #1 tell of AI-generated design. Use a 2-column staggered layout, an asymmetric grid, or horizontal scroll instead

### When DESIGN_VARIANCE ≥ 8
- Use CSS Grid fractional units (`grid-template-columns: 2fr 1fr 1fr`)
- Large areas of whitespace are allowed (a sense of space at the level of `padding-left: 20vw`)
- Masonry-style staggered layouts are allowed

### Cards and containers
- Only use a card when you genuinely need to separate an elevation layer
- Group data metrics with `border-top`, `divide-y`, or plain whitespace — don't box each one up individually
- Shadows must be tinted (matching the background tone), not the default gray shadow

## 5. AI-generation checklist of things to avoid

Before generating any visual content, go through this item by item:

### Visual & CSS
- No outer glow: don't use a default `box-shadow` glow. Use an inner border or a tinted shadow instead
- No over-saturated accent colors: the accent color should blend with the neutrals
- No custom cursor (not relevant for a static image, but also don't add it when generating HTML)

### Typography
- No Inter font: use Geist, Outfit, Cabinet Grotesk, or Satoshi
- Don't build hierarchy with font size alone: use weight and color too

### Content & data
- No generic placeholder names: John Doe, Sarah Chan, Jack Su are not allowed. Names should read like they were pulled at random from an address book
- No fake data: avoid `99.99%`, `50%`, `1234567`. Data should look like it was copied off a real record (`47.2%`, `+1 (312) 847-1928`)
- No cheesy startup names: Acme, Nexus, SmartFlow are not allowed. Come up with a name with some taste
- No AI-copywriting tone: "empower," "seamless," "unlock," "next-generation" are not allowed. Use concrete verbs
- No Unsplash links: for placeholder images use `https://picsum.photos/seed/{random-string}/800/600` or an SVG

### Spacing & alignment
- Calculate padding and margin to a grid; don't leave half-finished gaps
- Adjacent elements must align strictly, with visual lines running through cleanly

## 6. Materials and surfaces

### Glassmorphism
Frosted glass can't rely on `backdrop-blur` alone — layer it with:
- A 1px inner border: `border: 1px solid rgba(255,255,255,0.1)`
- A subtle inner shadow: `box-shadow: inset 0 1px 0 rgba(255,255,255,0.1)`
Only then do the edges refract light like real glass.

### Corner radius
- Use large rounded corners on the main container (`border-radius: 2.5rem`)
- Diffuse shadow (very faint, wide-spread): `box-shadow: 0 20px 40px -15px rgba(0,0,0,0.05)`

## 7. Final self-check

After generating the HTML and before taking the screenshot, confirm each item:

- [ ] Avoided a centered hero (when DESIGN_VARIANCE > 4)?
- [ ] Avoided three equal-width columns?
- [ ] Headings use a non-Inter font?
- [ ] Warm/cool tone is consistent, no pure black?
- [ ] Accent color ≤ 1, saturation < 80%?
- [ ] Data feels real (not fake like 99.99%)?
- [ ] No AI tone in the copy (empower/seamless/unlock)?
- [ ] Spacing is calculated precisely, no half-finished gaps?
- [ ] Shadows are tinted (not default gray)?
