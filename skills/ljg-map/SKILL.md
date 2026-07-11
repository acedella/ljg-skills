---
name: ljg-map
description: "One industry → one ecological terrain card (PNG). Grounded in the reference-frame theory from *A Thousand Brains* (《千脑智能》): flatten an industry into a bird's-eye-view 'ecological terrain' — value flows through the landscape like a river — then mark two spots on the terrain: the 'bottleneck' (瓶颈, the pass/dam where flow/capacity narrows) and the 'value-capture point' (价值捕获点, the treasure pile where profit settles). The terrain makes power structure visible at a glance: where flow is choked is often not where the money settles. Add three key-metric base rates (a ruler) + three 'big questions' (the frontier). Deep research goes online for real; the image is AI-generated (default -a Seiji Yoshida (吉田诚治)-style picture-book art, same lineage as ljg-library; optional -c pixel+cyber); Jigang (继刚) appears as a tiny surveyor standing on the terrain, looking out over it. Use when user says '行业地图' (industry map), '产业地图' (industry map), '生态地形图' (ecological terrain map), '画一下这个行业' (draw this industry), 'industry map', 'map this industry', '行业版图' (industry landscape), '产业链地图' (industry-chain map), '/ljg-map', or gives an industry/领域 name wanting its terrain mapped. Style: default -a picture-book (same as ljg-library), add -c for cyber. NOT FOR reducing a domain to its generators (use ljg-rank), breaking down a book (use ljg-book), single-project investment analysis (use ljg-invest), or deep-diving a single concept (use ljg-think)."
user_invocable: true
version: "2.1.0"
---

# ljg-map: Ecological Terrain Card

Input an industry, output a bird's-eye-view ecological terrain — where value flows, where it's choked, where money piles up into treasure, where the future battle will be fought.

## Foundation: Reference-Frame Theory (*A Thousand Brains*, 《千脑智能》)

Hawkins's view: the brain understands anything by pinning its features to a position on a reference frame (a map), and learns by moving around and predicting. The same goes for looking at an industry. Without a map, an industry sits in your head as a blurry list of company names; flatten it into an ecological terrain — a river of value flowing through a landscape, each segment its own terrain feature — and structure that was previously invisible becomes visible: where value flows, where it narrows, where profit settles.

So this isn't decorative scenery — it's structured terrain. Every landform's position needs an ecological reason — who's upstream, who's downstream, where the river narrows into a pass, where the treasure piles up. If a position can't be justified ecologically, it's just a pretty empty island — the reference frame was never actually built.

Thesis (must be on the card): **Only by flattening an industry into ecological terrain can you see — where the river narrows (the bottleneck) and where the gold settles (value capture). And these two spots are often not the same place.**

## Two Marked Spots, and Their Mismatch

What the whole card needs to accomplish: mark both spots on the terrain so they're instantly distinguishable, and make their mismatch or overlap visible. Deep research pins them down first, then translates them into terrain.

### Bottleneck (瓶颈) (where the river narrows → pass / dam / single-log bridge)

The segment of the industry that's choked — where capacity, flow, or control narrows. Signals to identify it:

- **Scarce capacity**: expanding takes years and massive capital (fabs, lithium mines)
- **Single-point control**: one or two players hold the chokepoint, and there's no way around them (ASML's lithography machines, Google's search entry point)
- **License/patent/standard gate**: a legal or technical-standard checkpoint (drug approval, the CUDA ecosystem)
- **Physical/cognitive limit**: bandwidth, latency, attention, trust — a ceiling that can't be gotten around

One-line test: **to scale up output in this industry, which segment gets choked first?** Draw that segment as a narrowing pass or dam, and pin a red flag on it.

### Value-Capture Point (价值捕获点) (where profit settles → treasure pile / vault)

The segment where the money actually stays. Creating value and capturing value are two different things. Signals to identify it:

- **Bargaining power**: both upstream and downstream have to come to it, and it calls the shots (operating systems, platforms collecting rent)
- **High and durable margins**: high profit margin that's also defensible (brands, patented drugs, network effects)
- **Winner-take-all**: scale or network effects let the leader eat most of the profit

One-line test: **where does the money this industry earns finally settle?** Draw that segment as a pile of gold treasure, and pin a gold flag on it.

### Mismatch and Overlap

Bottleneck and value capture are two independent quantities — find each separately first, then see where they land on the terrain:

- **Overlap**: whoever holds the chokepoint is also the one making the money (ASML, TSMC) — the treasure pile sits right next to the pass, power and profit share the same source.
- **Mismatch**: the segment that creates value sits on thin soil, while the profit is siphoned off elsewhere (creators farm the village while the platform's harbor collects the rent; hardware's field is thin while Nvidia's dam piles up a mountain of gold).

The caption should nail the terrain's power structure in one line (e.g. "double-headed rent extraction: two treasure piles front and back, the village in the middle just farms and collects nothing"). Mismatch is the single most valuable finding this kind of card can produce — a power structure turned into a visible mountain of gold and a strip of thin soil on the terrain.

## Three Big Questions

Exactly three questions that are still hanging — tension still live, answer undetermined, and once settled it rewrites the landscape. Usually found at the terrain's frontier: will the pass get bypassed by a new route? Will the treasure migrate to another plot of land? Which new force is redirecting the river? One sentence each, debatable, future-facing, and the three shouldn't overlap. Criteria in `references/research.md`.

## Base Rate: the Terrain's Ruler

Terrain gives structure, but without a ruler it gets skewed by anecdotes — see one unicorn and think gold is everywhere. The base rate is the ruler: the baseline value of the industry's three most critical metrics, the prior you should hold absent extra information. Three things together make a complete reference frame: terrain handles "where," base rate handles "how much," the three big questions handle "what's next." Read the card in that order too. Each base rate has three fields: metric name + value (range/percentage, in large type) + one line of "here's what this means," nailing down the illusion it dispels. Criteria: decision-relevant, counter-intuitive, verifiable — real numbers, not made up. Details in `references/research.md`.

## Style Molds

| mold | flag | style |
|------|------|-------|
| **Seiji Yoshida (吉田诚治) picture-book map (default)** | `-a` | Same lineage as ljg-library: warm hand-painted picture-book oil style, honey/amber/wood-brown, low-angle slanting sunlight, textured bird's-eye terrain, handwritten wooden signposts; not cartoon, not pixel |
| **pixel + cyber-hacker** | `-c` | Deliberately breaks style: dark neon, 16-bit pixel terrain, CRT/glitch, terminal font |

The default `-a` is family with ljg-library (same Seiji Yoshida picture-book DNA), just with the composition switched to a bird's-eye terrain map; `-c` is a deliberate cyber departure from house style. Both are generated by `assets/gen_illustration.py --mold a|c`; Jigang (继刚) always appears as the tiny surveyor standing at the highest point looking out over the terrain (generated from an ink-portrait reference, recognizable).

## Workflow

```
Input: industry/domain name (user's own judgment can be attached) [+ -a/-c to pick mold, default -a]
  ↓
1. Deep research: value-chain structure, key segments, profit distribution, chokepoints, open questions + three base rates (see references/research.md)
2. Pin down the two spots: 🔴 bottleneck (segment where output chokes first) + 🟡 value capture (segment where money finally settles); check mismatch vs. overlap
3. Translate the research into a terrain frame (English): how the river flows, what landform each segment is, where the pass is, where the treasure is, where Jigang stands, 3-6 Chinese place-name labels (see references/visual.md + example.md)
4. Generate the image: python3 assets/gen_illustration.py --mold a --frame "<...>" --out /tmp/ljg_map_{slug}_terrain.png
5. Distill three base rates (metric + value + "what this means")
6. Distill three big questions
7. Pick an accent color (for the picture-book mold, pick warm honey/amber/wood-brown tones, see visual.md)
8. Fill in the placeholders in assets/map_template.html ({{MAP_IMG}} = file:// to the generated image)
9. Render (capture.js, fullpage)
10. Read the finished PNG and inspect it with your own eyes (check against visual.md's factory checklist); if it's off, adjust the frame and regenerate → deliver
```

## Two Reference Docs: Research and Visual

- Before executing, Read `references/research.md` — the five groups of questions to ask when researching an industry, the steps for identifying bottleneck and value capture, the criteria for distilling the three big questions and base rates, and the discipline for fanning out research agents.
- Before generating, Read `references/visual.md` — the glass-card body spec, the palettes for both molds, how to translate research into a terrain frame, and the factory self-check. Then look at `references/example.md` — a fully-rendered, verified worked example (AI film/video, double-headed rent extraction), whose frame you can adapt and reuse.

## Template Variables (map_template.html)

| Variable | Content |
|------|------|
| `{{ACCENT}}` | Card-body accent color hex (for picture-book mold, pick warm honey/amber/wood-brown, e.g. `#c47a1a` / `#b8860b` / `#725d42`; cyber mold picks neon) |
| `{{INDUSTRY}}` `{{EN}}` | Industry name in Chinese / English |
| `{{TAGS}}` | 3-4 core segment/concept tags, each as `<span class="tag">…</span>` |
| `{{THESIS}}` | The reference-frame thesis sentence (only by flattening into terrain can you see both spots, and they're often mismatched), with keywords in `<span class="hl">` |
| `{{MAP_IMG}}` | Absolute `file://` path to the ecological terrain image |
| `{{MAP_CAPTION}}` | One line under the image: nails the mismatch/power structure |
| `{{BASE_RATES}}` | Three base rates: each `<div class="brate"><div class="blabel">metric name</div><div class="bval">value</div><div class="bmean">what this means…</div></div>` |
| `{{Q1}}` `{{Q2}}` `{{Q3}}` | The three big questions, one sentence each |

## Rendering

```bash
node ~/.claude/skills/ljg-card/assets/capture.js \
  /tmp/ljg_map_{name}.html ~/Downloads/{name}-生态地形图.png 1080 1440 fullpage
```

Reuses ljg-card's capture.js (playwright already installed). Don't drop `fullpage` — card height follows content.

## Delivery

1. Read the finished PNG and inspect it with your own eyes; zoom in on the terrain image and go through the visual.md factory checklist item by item. If the generated image is unsatisfactory (place names blurry, the two spots not clearly marked, Jigang doesn't look right), adjust the frame and regenerate.
2. Report: file path + one sentence pointing out this terrain's most important finding (usually the mismatch) + which mold was used.

## Must-Pass Checks

1. **It's ecological terrain, not decorative scenery** — every landform's position must have an ecological reason (upstream/downstream, how the river flows, who chokes whom, where the gold piles up). If you can't say why, go back and retranslate.
2. **Both spots must be marked and clearly distinct** — 🔴 bottleneck (pass/dam, red flag), 🟡 value capture (treasure pile, gold flag).
3. **Mismatch/overlap must be called out** — the caption must state the power structure plainly.
4. **Exactly three big questions** — undecided, future-facing, non-overlapping.
5. **Deep research, not impressions** — the value chain, profit distribution, and chokepoints must be verified online. Segments made up from thin air won't survive the re-check of "does this segment actually choke, does it actually make money."
6. **Base rates must be real, not invented** — ranges are fine, fabrication is not. A made-up prior is worse than no prior at all.
7. **Numbers and questions don't go into the image** — base-rate numbers and the three big questions live in the card's text blocks; the image only carries the terrain + the two marked spots + a few place names.
8. **Real render, real inspection** — must render via capture.js and must Read the PNG with your own eyes; never hand it in on the claim of "already generated."
9. **The Chinese text on the card must pass muster** — read the thesis/caption/three big questions aloud silently, and rewrite anything that sounds like translationese; don't use self-congratulatory rhetoric like "这一刀" (this cut), "锋利" (sharp, as meta-commentary), "钉死" (nailed down), "砸实" (hammered solid).
10. **Same house style lineage** — light-colored glass card body + embedded generated image, family with ljg-library.

## Gotchas

- **Degenerating into decorative scenery**: the most common failure. Ask yourself "where does the river flow, who's upstream, who does the pass choke, where does the gold pile up" — if you can't answer, it's an empty island.
- **Defaulting to assuming the two spots overlap**: bottleneck and value capture must be checked independently, then compared for where they land — mismatch is the real insight; defaulting to overlap erases it.
- **Drawing segments as companies**: landforms = functional positions (wafer-foundry mountains, model-training valley, distribution harbor); companies are just the current occupants, shown as small place-name labels. Occupants change, the landform stays.
- **Cramming precise detail into the image**: image generation can't handle many labels or long numbers well. Keep place names to 3-6, each 2-5 characters (long names get blurry); put numbers and questions entirely in HTML text blocks.
- **Using semicolons or Chinese enumeration commas to separate place names in the frame**: separating Chinese labels with ` / ` gets falsely flagged BLOCK by the safety hook — don't use a slash.
- **Must look at the generated image**: the same frame produces a different image every time — Chinese place names may blur, the two spots may not be clearly marked, Jigang may not look right. Always Read the generated image and inspect it; if it's off, adjust the frame and regenerate — save the current one to a different path before regenerating.
- **Accent is the card-body color**: pick it from the mold's palette (picture-book: warm honey/amber/wood-brown; cyber: neon); the terrain image's own color scheme is separate and the two shouldn't clash.
- **/tmp filenames should include the industry slug**: when minting cards in parallel, temp HTML and generated images must use unique names — sharing a fixed name will mix up images.
- **Batch pipeline**: `gen_illustration.py` calls marswave directly, bypassing listenhub's interactive gating, so it can run in batch; each image still needs network access, spends API budget, and needs a real visual check — leave slack in scheduling.
- **Research fan-out**: concurrent research agents must return synchronously — never leave a Monitor dangling in the background; stagger batches; sample-check key numbers independently before they're committed.
</content>
