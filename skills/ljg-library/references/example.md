# Frame examples

Two complete commands you can adapt directly. The frame only describes what to paint — Jigang (继刚) as protagonist's composition, the metaphorical objects, the Chinese labels; style words aren't written, since `gen_illustration.py` already has the Seiji Yoshida (吉田诚治) style built in, and writing them would conflict.

## Example 1 · *Antifragile*/*Skin in the Game*-style ergodicity

Mental image (see extraction.md example A for the distillation process): a crowd each plays once, averaging +5%; "you" walk alone along a single timeline, and along the way there's an absorbing wall — go in and you can't get out — where even a positive expectation leads to ruin. Jigang is that "you" walking the timeline.

```bash
python3 assets/gen_illustration.py \
  --frame "In the FOREGROUND the protagonist walks alone along a single narrow wooden boardwalk lit by low amber sunlight, heading toward a dark swirling whirlpool that swallows everything at the end of the planks (an absorbing trap, fall in and never climb out). In the BACKGROUND, on safe flat ground beside warm cottages, a crowd of small figures strolls calmly on a wide level path. Labels: 玩很久 (near the man on the boardwalk); 各玩一次 (near the crowd); 吸收壁 (small sign by the whirlpool); 出不来 (under the whirlpool)." \
  --out /tmp/ljg_lib_ergodicity_sketch.png
```

Resulting image: Jigang walking a warm-lit wooden boardwalk toward a whirlpool that swallows everything, with a crowd on a safe flat path in the distance, and wooden signs bearing the labels.

## Example 2 · *A Thousand Brains* reference-frame voting

Mental image: a brain is not a single observer but thousands of cortical columns each drawing a "reference frame" map of the same object to model it — the "one thing" you perceive is a consensus voted on by thousands of maps. Jigang = the column in the foreground currently drawing a map.

```bash
python3 assets/gen_illustration.py \
  --frame "A warm circular library-hall, cozy and lived-in with wooden beams, bookshelves and plants. In the FOREGROUND the protagonist sits at a small wooden desk under a glowing brass lamp, carefully drawing a grid/coordinate MAP of a single coffee cup that stands on his desk. Behind and around him the SAME scene repeats into the distance — tier upon tier of hundreds of identical little lamplit desks, each with a small figure drawing his own map of the SAME coffee cup. Soft glowing lines rise from all the desks and converge upward onto ONE large clear coffee cup floating above the centre of the hall, as if all the maps are voting to agree on it. Warm low sunlight, honey and amber tones. Labels: 参考系 (on the map he is drawing); 千根柱子 (across the tiers of desks); 投票共识 (near the converging lines); 一个杯子 (under the floating cup)." \
  --out /tmp/ljg_lib_qiannao_sketch.png
```

Resulting image: a warm circular library hall, Jigang in the foreground drawing a reference-frame grid map, tiers of small lamplit desks behind him each drawing the same cup, light converging upward onto a floating coffee cup at the top. This image was actually generated and reviewed: labels all correct, Jigang recognizable, hall atmosphere on point.

## Reuse notes

- Fix the protagonist first: where Jigang is, which way he's facing, what core action he's performing. His role is assigned by the picture — he isn't necessarily the hero (see extraction.md example B).
- Ground the metaphor: turn abstract propositions into visible objects — planks, bridges, whirlpools, driver's seats, old cars.
- Spell out the contrast and direction of flow: who's on the safe side, who's on the dangerous side, where lines converge.
- 3-5 labels, each ≤5 characters, specifying which element each is attached to, separated by semicolons or Chinese enumeration commas (` / ` gets blocked by the safety hook).
- Style words aren't written — already built in.
- Always Read and verify after generating: is Jigang recognizable, does the picture read clearly, is the Chinese text not blurry; if not, adjust the frame and regenerate — save the current image first before regenerating.
