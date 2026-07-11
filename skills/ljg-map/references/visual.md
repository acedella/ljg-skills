# Visual Spec: Card Body, Molds, Terrain-Frame Translation

Read this through before generating. Card = light-colored optical-glass card body (hard-coded in the template, don't touch) + one embedded AI-generated ecological terrain image.

## Glass Card Body (hard-coded, do not touch)

| Role | Value |
|------|-----|
| Background gradient | `#eef1f7 → #e6ebf3 → #e3e8f2` cool misty white |
| Glass card surface | `rgba(255,255,255,0.55)` + `backdrop-filter:blur(40px) saturate(1.3)` |
| Highlight edge | `1px solid rgba(255,255,255,0.85)` |
| Soft shadow | `0 8px 32px rgba(31,41,72,0.12)` + `0 40px 80px -32px …` |
| Body text | `#1a1f2e` ｜ secondary `#5b6472` |

Keep the card body light-colored; the terrain image brings its own background (picture-book honey/amber warm tones, or cyber dark) and is the only heavy-color block on the card — this contrast in tone is deliberate.

## Accent Color {{ACCENT}}

Used for the kicker, tags, the English line, thesis keyword highlights, the big-question numbers, the base-rate numbers, and the signature stamp. For the picture-book mold, pick warm honey/amber/wood-brown tones (`#c47a1a` / `#b8860b` / `#725d42`) to match Seiji Yoshida's (吉田诚治) warm terrain; for the cyber mold, pick neon (indigo/cyan). Neither should clash with the terrain image's own palette.

## Fonts (card-body text)

thesis / big questions / industry name → Noto Serif SC; tags / EN / base-rate numbers → JetBrains Mono. Place names inside the terrain image are baked into the generated image itself (picture-book handwritten wooden-sign lettering, or cyber terminal font) and aren't controlled by CSS.

## Structure

```
Identity block: ecological terrain kicker | industry name + EN + tags
─────
Reference-frame thesis: {{THESIS}}
─────
Ecological terrain block: AI-generated terrain image {{MAP_IMG}} (16:9, bottleneck 🔴 + value capture 🟡 marked on the terrain)
              {{MAP_CAPTION}} one-line caption under the image, nailing the mismatch/power structure
─────
Base-rate block: {{BASE_RATES}} three columns (metric / big number / what this means)
─────
Three-big-questions block: {{Q1}}{{Q2}}{{Q3}} ①②③
─────
Signature: seal — Li Jigang (李继刚)
```

Image and text each carry their own load: the image carries the atmosphere, the two marked spots, and a few place names; base-rate numbers and the three big questions live in the card's text blocks. Image generation can't handle many labels or long numbers well — this division of labor is the fix for that.

## The Two Molds

The terrain plate `.mapplate` is a framing container (`border-radius:16px; overflow:hidden`) that holds the generated 16:9 PNG. The style DNA is hard-coded in `assets/gen_illustration.py`:

| mold | `--mold` | style |
|------|----------|------|
| **Seiji Yoshida (吉田诚治) picture-book map (default)** | `a` | Same lineage as ljg-library: warm hand-painted picture-book oil style, low-angle slanting sunlight; honey/amber/wood-brown `#c47a1a`/`#725d42`, paired with soft teal water-plants; textured bird's-eye terrain, handwritten wooden place-name signs; not cartoon, not pixel |
| **pixel + cyber-hacker** | `c` | 16-bit pixel terrain + cyber-hacker; dark background + CRT/glitch; neon green/cyan/magenta; terminal-font place names |

The script's COMMON block already bakes in "bird's-eye ecological terrain + river of value + bottleneck red flag (pass/dam) + value-capture gold flag (treasure pile) + Jigang as a tiny surveyor." The frame only needs to lay out this particular industry's terrain.

## How to Translate Research into a Terrain Frame

Once deep research has produced "value chain + which segment is the bottleneck + which segment is value capture," write a paragraph of English terrain composition:

- **River of value**: value flows from upstream to downstream, a river or road winding from one end of the terrain to the other.
- **Each segment = a landform**: upstream raw materials/equipment are mountain mines, manufacturing is a workshop-and-furnace valley, platforms/channels are harbor towns, end-users/consumers are the open sea or a city. Segments are functional positions; companies are shown only as small place-name labels.
- **Bottleneck**: a narrowing pass, a dam, a single-log bridge — where the river gets choked.
- **Value capture**: a pile of gold treasure, a vault — where the money settles. A mismatch places the treasure far from the thin soil that created it; an overlap places the treasure right next to the pass.
- **Jigang**: the tiny surveyor stands at a high point looking out (already in COMMON; the frame can specify where he stands).
- **3-6 place names**, each 2-5 characters, noting which landform each hangs beside, separated by semicolons or Chinese enumeration commas (` / ` gets blocked by the safety hook). Too many or too long, and they blur.

The industry's structure decides the layout: supply-chain type gets a river valley, platform type gets a central island, layered type gets terraced hillside (criteria in research.md).

## Factory Self-Check

- [ ] Are the bottleneck pass 🔴 (red flag) and the value-capture treasure 🟡 (gold flag) instantly distinguishable, and correctly positioned?
- [ ] Is the river of value's direction clear (upstream→downstream)? Can the mismatch/overlap be read off?
- [ ] Is Jigang the surveyor recognizable (glasses, beard), without stealing the terrain's spotlight?
- [ ] Are the Chinese place names correct and not blurry? (If blurry, cut place names and regenerate the frame.)
- [ ] Is the image actually the terrain described by the thesis/caption?
- [ ] Are the base-rate three-column block and the three big questions in the card's text block, and clear?
- [ ] Is the card-body accent color used only as an accent (warm honey/amber/wood-brown for picture-book)?
- [ ] Is the text aligned on both sides, no whitespace on the right, height auto-adjusting, no blank space at the bottom (fullpage)?

Fail any of these — go back to SKILL.md's Gotchas for the corresponding fix.
</content>
