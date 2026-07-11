# Mental image: how to distill it, how to paint it

This file handles two things: distilling the mental image from the book (part one), and writing it into a frame to generate the illustration (part two).

First, let's be clear about what's being distilled. Viewfinder (取景框) = angle + question + picture: the author stands at a certain position, looks at a certain question, and sees a picture that others hadn't seen. This mental image is the center of the whole card — the text explains it, the illustration paints it; both are two renderings of the same picture. It's what the reader remembers six months later.

Explaining the picture clearly matters more than explaining it briefly. There's no word-count ceiling — as long as the picture is concrete and the explanation is accessible and accurate, that's enough.

## Part 1: Distillation (six steps)

1. **Subject**: what question is this book looking at?
2. **Angle**: from what position, with what perspective, does the author look at it? The "framing" (取景) of the viewfinder happens at this step.
3. **Old picture**: before this book, what picture did ordinary people and various schools of thought see from the usual angle?
4. **New picture**: switching to the author's angle, what new picture do you see? It needs to be concrete, paintable, expressible in one sentence. The gap between it and the old picture is this book's entire value. Test whether it's a picture or a proposition: can it be described as a painting — with visible objects, visible actions, visible contrast? If you can't describe a picture, it's still stuck at an abstract proposition — go back to step 2 and find a different angle.
5. **Feynman explanation**:
   - The explanation must be generated via the `feynman-eli5` skill, not hand-written — this is Jigang's (继刚) fixed rule. Feed it: the new picture, the old picture, the mechanism (why looking from this angle reveals this picture); what should come back is an accessible, accurate explanation with a concrete analogy, free of jargon.
   - `{{FRAME}}` = one sentence stating how this picture changes one's worldview, in the pattern "it's not X, it's actually Y," with an action verb. Don't write a noun definition, don't write "what this book is about." Color keywords with `<span class="hl">`.
   - `{{EXP}}` = the feynman-eli5 output: what picture this is, why looking from this angle reveals it, and how it changes what you see. Color keywords the same way.
6. **Verification**, three checks:
   - Remove the book title and author — can `{{FRAME}}` + `{{EXP}}` stand alone as a way of seeing the world? If it's just a summary, redistill it.
   - Run it through the five-question translationese check (subject, verb, preposition, modifier, read-aloud test) — it should read like natural language.
   - Do the picture the text describes and the picture the image depicts match? If not, align them before delivering.

The ultimate test: after your mind has cooled down, does glancing at the card bring the picture back?

### Example A · *Antifragile*/*Skin in the Game*-style ergodicity

- Subject: a gamble with positive expected value — should you take it?
- Angle: instead of standing on "the average of a group each playing once," stand on the timeline of "you playing for a very long time."
- Old picture: a hundred parallel versions of you spread out flat; if expectation is positive, you should take the bet.
- New picture: two fates diverge — the crowd advances at a steady pace on flat high ground (+5%), while "you" walk alone along a single timeline toward an absorbing wall (bankruptcy, death — go in and you can't get out), where even a positive expectation leads to ruin.
- `{{FRAME}}`: `Whether a gamble is worth taking isn't judged by the average of "many people each playing once" — it's judged by the fate of "you alone <span class="hl">playing for a very long time</span>."`
- `{{EXP}}` (feynman-eli5 output): `Classical decision theory judges by <span class="hl">ensemble average</span> — spread a hundred parallel versions of you out flat, and if expectation is positive, you should take it. But you don't live in the average of parallel universes — you walk only <span class="hl">one timeline</span>; and if there's an <span class="hl">absorbing wall</span> along the way (bankruptcy, death, collapse — go in and you can't climb out), a positive expectation will still steadily push you toward ruin. So what really determines your fate is the time average, not the ensemble average.`
- Verification: stands alone without the title; reads naturally aloud; text and image depict the same picture.
- Illustration frame: Jigang walking alone along a warm-lit wooden boardwalk toward a whirlpool that swallows everything at the end, with a crowd on a safe flat path in the distance (full command in example.md).

### Example B · *The Selfish Gene*

- Subject: why has life turned out the way it has? Who is the protagonist in evolution?
- Angle: instead of standing at the position of the individual or the species, crouch down at the position of the gene and look up.
- Old picture: humans are the protagonist, genes are a tool for passing on the lineage.
- New picture: reversed — a stretch of gene sits in the driver's seat, and the body is a car it built and discards once used; generation after generation of cars are born and scrapped, while the same gene passes through them, moving forward.
- `{{FRAME}}`: `It's not <span class="hl">you</span> using genes to pass on your lineage — it's the <span class="hl">gene</span> using this body of yours to replicate itself.`
- `{{EXP}}` (feynman-eli5 output): `Think of a gene as a set of instructions for "how to build a body." Whichever set of instructions builds a body that survives better and prints another copy better, that set of instructions is the one that gets passed on. So the body isn't the goal — it's the <span class="hl">means</span> by which the instructions print themselves; your entire life is essentially a <span class="hl">temporary car</span> assembled by an ancient set of instructions, carrying it one more leg of the journey — when the stop comes, you change cars, and the instructions keep going.`
- Illustration frame: Jigang is still in the picture, but in the passenger seat — in the driver's seat is a personified strand of DNA gripping the wheel, with a row of scrapped old cars by the roadside. Jigang's role in the picture is whatever position the mental image assigns to "you" — not necessarily the hero; the picture's irony only works precisely because "he's the one being driven."

## Part 2: Generating the illustration

Tool: `assets/gen_illustration.py`. It feeds `assets/ljg-portrait.png` (Jigang's ink portrait) as a character reference to the model, painting a recognizable him; the Seiji Yoshida (吉田诚治) style is already built into the script. So the frame only needs to describe what to paint, not style words — writing them would actually conflict with the built-in style.

### Writing the frame

The frame is an English composition description that turns the mental image into a concrete, paintable scene, covering four things:

- Where Jigang (you) is, what he's doing: the core action and position. His role is assigned by the picture, as in example B.
- Real metaphorical objects: planks, whirlpools, driver's seats, old cars, bridges — abstract propositions can't be painted, they must become visible objects.
- Contrast and direction of flow: who's on the safe side, who's on the dangerous side, which way information travels.
- 3-5 Chinese labels, each ≤5 characters, specifying which element each is attached to. Separate labels with semicolons or Chinese enumeration commas — ` / ` gets misjudged and BLOCKed by the safety hook. Let `{{EXP}}` carry mechanistic detail in the body text; don't cram it into the labels.

After writing, ask yourself: if this frame were painted, would it be the same mental image from part 1? If yes, generate the image.

### Generating the image

```bash
python3 assets/gen_illustration.py \
  --frame "In the FOREGROUND the protagonist walks alone along a single narrow wooden boardwalk tilting toward a dark swirling whirlpool that swallows everything at its end (an absorbing trap, fall in and cannot climb out). In the BACKGROUND, on safe flat ground, a crowd of small figures strolls calmly on a wide level path. Labels: 玩很久 (near the man); 各玩一次 (near the crowd); 吸收壁 (sign by the whirlpool); 出不来 (under the whirlpool)." \
  --out /tmp/ljg_lib_ergodicity_sketch.png
```

Fill the output PNG's absolute `file://` path into `{{SKETCH_IMG}}`; `{{SKETCH_TITLE}}` is the image's name (English + Chinese, e.g. `Ergodicity 遍历性`).

### Verification

Every generation produces something different, and the model can blur Chinese text, paint the person unrecognizably, or get the action wrong. After generating, always Read and verify four things: Jigang is recognizable, the picture reads at a glance, the Chinese labels are correct and not blurry, and it matches `{{FRAME}}`/`{{EXP}}` as the same picture. If it doesn't pass, adjust the frame and regenerate — shorter/fewer labels, clearer protagonist action, more concrete metaphor tend to work best. Save the current image to a different path before regenerating — the new one isn't necessarily better.
