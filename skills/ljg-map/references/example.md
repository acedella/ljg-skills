# Worked Example: AI Film/Video Ecological Terrain Map

A complete example you can adapt. The frame only needs to describe how this particular industry's terrain is laid out (the river of value, each segment's landform, the pass, the treasure, the place names); style, the red/gold flags, and Jigang the surveyor are all baked into `gen_illustration.py` and don't need to be written.

## Research → Terrain

The structure deep research produced (double-headed rent extraction): value chain compute → base video model → tools → content production (creators) → distribution platform → viewers; bottleneck = the base video model (the gate on capacity and compute, burns cash); value capture = compute (Nvidia) + distribution platform (Douyin/Kuaishou), both ends; mismatch = creators create value in the midstream but hold thin soil, while the money is siphoned off by both ends.

Translated into terrain: upstream compute snow mountains plus a big dam (treasure); midstream a green valley of creator-villagers with content fields (thin soil); a canyon pass just above the midstream = the bottleneck; downstream a platform harbor town with treasure; the open sea = the viewers.

## Mold -a (Seiji Yoshida picture-book map, default)

```bash
python3 assets/gen_illustration.py --mold a \
  --frame "A value-river flows left-to-right through an island valley. LEFT/upstream: tall compute mountains with a big wooden dam-tollgate (a major profit pool). MIDDLE: a lush green valley where small creator-villagers grow content crops by the river — value is created here but the villagers are modest. A narrow rocky canyon pass sits just upstream of the middle (a scarce, smoking foundry-hut zone = the base-model layer) constricting the river. RIGHT/downstream: a busy harbor town with a tollgate where boats pay before reaching the open sea (the audience) — another profit pool. So the bottleneck pass is the canyon; the value-capture treasure piles are the upstream dam and the downstream harbor. A few wooden place labels: 算力; 创作者; 平台; 观众." \
  --out /tmp/ljg_map_aifilm_terrain.png
```

Resulting image: compute snow mountains with a gold treasure pile on the dam, midstream creator village with content fields, red-flagged canyon bottleneck, platform harbor town with treasure, open sea of viewers, Jigang as the surveyor standing on a hilltop holding up a map, looking out. This image was actually generated and passed rendering review.

## Mold -c (pixel + cyber-hacker)

The same frame, just swap in `--mold c`, and out comes a dark neon pixel version (the canyon pass becomes a glitch-narrowed chokepoint, the treasure becomes a glowing data pile, the river becomes a data stream). The frame doesn't change — the mold handles the style.

## Key Takeaways for Reuse

- Do the research before translating to terrain: pin down which segment is the bottleneck, which is value capture, and whether it's a mismatch or overlap, before drawing anything.
- The river of value runs from upstream to downstream; translate each segment into a mountain, valley, harbor, or sea.
- Draw the bottleneck as a narrowing pass or dam, and value capture as a treasure pile; if it's a mismatch, place the treasure far from the thin soil that created it.
- 3-6 place names, each 2-5 characters, separated by semicolons or Chinese enumeration commas (` / ` gets blocked by the safety hook).
- Numbers and questions don't go in the image: base-rate numbers and the three big questions go in the card's text blocks.
- Always Read and inspect the generated image: both spots clearly marked, place names not blurry, Jigang recognizable; if not, adjust the frame and regenerate — save the current image to a different path first before regenerating.
</content>
