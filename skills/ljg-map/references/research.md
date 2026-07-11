# Research Playbook: Building the Map, Finding the Two Spots, Distilling the Three Questions and Three Rates

Before the card can look good, the industry research has to be right. This file governs the research: what to look up, how to fan it out, how to place segments on the terrain, how to pin down the two spots precisely, and how to distill the three big questions and three base rates. If the research fails, the card degenerates into an illustrated industry encyclopedia entry.

The goal of the research isn't to pile up facts — it's to find the industry's true coordinate system: which direction value flows, where each segment is pinned, where flow narrows, where profit settles. Once you have the map, structure that was previously invisible becomes visible — this is the reference-frame theory from *A Thousand Brains* (《千脑智能》), applied to industry analysis.

## I. Deep Research: What to Look Up

Research online (via the Research / web-access skill), organized around five groups of questions, each grounded in a specific "segment":

1. **Value-chain structure**: from raw input to end user, which segments does value pass through? Who feeds whom?
2. **Players in each segment**: who currently occupies each segment? Concentrated or fragmented? Players are occupants; the segment itself is the functional position.
3. **Profit distribution**: where does the money finally settle? Which segment has high, defensible margins? Which segment is thankless grunt work? — evidence for value capture.
4. **Capacity/flow chokepoints**: to scale up output, which segment chokes first? Who finds it hard to expand capacity, who holds single-point control, who has set up a gate? — evidence for the bottleneck.
5. **Unresolved tensions**: what's the industry's biggest current debate, variable, or turning point? What would rewrite the landscape once settled? — material for the three big questions.

If the user has already given their own read on the industry, treat it as a prior: research should verify, complete, and challenge it. For well-established industries you can draw on the model's own knowledge, but always ask of every item: is this something I looked up, or something I made up? Cut what's made up.

## II. Fan-Out Discipline

When running concurrent research agents, keep all three rules:

- **Synchronous return**: every agent's prompt must explicitly require a synchronous result — never leave a Monitor dangling in the background.
- **Batch + stagger**: when hitting rate-limited APIs, batch requests and stagger their timing — don't slam them all at once.
- **Sample-check**: before committing to disk, independently sample-check key facts (who occupies which segment, what the margin is, who holds the chokepoint) — never let a number from a single source go straight onto the card.

The five question groups can be split across five parallel explorers, each covering one group, then merged and deduplicated.

## III. Terrain Layout (Determined by Industry Structure)

How value moves determines how the river flows and how the landforms are arranged:

| Industry type | Terrain layout | River of value | Typical location of the two spots |
|--------|----------|----------|----------------|
| **Supply-chain/manufacturing type** (semiconductors, new energy, coffee, pharma) | River valley: upstream mountains → downstream open sea | One river flows left→right through the valley | Bottleneck = a narrowing pass/dam upstream; value capture = the treasure on the land holding the bargaining power |
| **Layered technology type** (software, AI, cloud) | Terraced hillside: bottom-layer infrastructure → top-layer application | A river/road winds up the slope from bottom to top | Bottleneck = a key bottom-layer step; value capture = the treasure at a high-margin layer |
| **Platform/two-sided type** (e-commerce, delivery, social, payments) | Central island + multi-sided docks | Multiple rivers converge toward the central rent-collecting harbor | Bottleneck = the chokepoint in and out of the center; value capture = the central harbor's vault |

Criteria: if value "flows through," use a river valley; if it "stacks up," use a terraced hillside; if it "connects together," use a central island. If unsure, default to a river valley.

## IV. Translating Segments into Landforms

Translate each key segment (functional position) turned up by the research into a landform — upstream raw materials/equipment become mountain mines, manufacturing becomes a workshop-and-furnace valley, platforms/channels become harbor towns, end-users/consumers become the open sea or a city. The landform is the functional position; the company is just the current occupant, shown as a small place-name label. Every landform's position must have an ecological justification: who's upstream on the river, who's closest to the open sea, where the river gets choked.

Test: can every key segment be mapped onto a corresponding landform with a justified position? If it can't fit, the value chain hasn't been thought through yet — go back to step one.

For the concrete drawing (pass, treasure, surveyor), see `visual.md`.

## V. Check the Two Spots Separately, Then See Where They Land

Don't assume the bottleneck and value capture coincide — find them independently:

- **First pass, find the bottleneck** (🔴): ask "which segment chokes output first," and check each segment against SKILL.md's four signal types (scarce capacity, single-point control, license/patent/standard gate, physical/cognitive limit).
- **Second pass, find value capture** (🟡): ask "where does the money finally settle," and check each segment against three signal types (bargaining power, high and durable margins, winner-take-all).
- **Then see where they land**: same segment = overlap (treasure sits next to the pass, caption nails it as "whoever holds the chokepoint makes the money"); different segments = mismatch (treasure sits far from the thin soil that created it, caption nails it as "value is created here, money is collected there").

Mismatch is the single most valuable finding on this card — prioritize fleshing it out fully; it's the industry's power map.

## VI. Three Big Questions

Once the map is drawn, distill exactly three questions that will determine the industry's future. Four criteria, all must pass:

- **Unresolved**: the answer isn't settled yet. Foregone conclusions don't count.
- **High-leverage**: once settled, it rewrites the landscape — the flow direction shifts, the bottleneck gets bypassed, the value pool migrates.
- **Debatable**: smart people split into two camps over it — not a trivia question.
- **Future-facing**: asks "will it, who will, when will," not "what is it."

Where to look — the map's frontier:

- Bottleneck frontier: will this chokepoint get bypassed by new technology?
- Value-pool frontier: will profit migrate away from the current segment? To where?
- Flow-direction frontier: which new player or new model is rewriting the direction value flows?
- Boundary frontier: will this industry merge with an adjacent one, or get absorbed by it?

The three should not overlap, and together should cover the industry's most critical uncertainty. Test: show it to an industry insider — would they say "yes, this is exactly what we argue about every day"? If what you've got are three pieces of textbook common knowledge, you haven't found it — try again.

## VII. Three Base Rates

The map gives structure; the base rate gives scale — the baseline value of the three most critical metrics, the prior you should hold absent extra information. This is Bayesian reasoning applied: the prior comes before the anecdote.

**Choosing the three: all three criteria must pass**:

- **Decision-relevant**: knowing this number would change how you bet.
- **Counter-intuitive**: it diverges from the popular impression. A base rate's value is proportional to the gap between it and the popular impression.
- **Verifiable**: a real number (or range) can be found — don't make it up.

**Each has three fields**: metric name + baseline value (range/percentage, large type) + one line of "here's what this means" — nailing down the illusion it dispels.

**How to find them**: ask "in this industry, what are the three numbers outsiders are most likely to be misled by from a single anecdote" — success/survival rate (startups, new drugs, restaurants), share of the split (coffee farmers get 1-3% of retail price), cost/price curves (year-over-year drop in training cost), concentration (how much the top few players eat up). Pick the three with the biggest gap from popular impression.

**Verification**: numbers should be traceable to a source, ranges expressed honestly ("~10%," "1-3%," "$100M+"), cross-checked across multiple sources. A made-up base rate is worse than none at all — it masquerades as a prior when it's really an illusion.

## VIII. Jigang the Surveyor

Jigang (继刚) always appears as the tiny surveyor standing at a high point on the terrain, looking out over this industry's ecology (generated from an ink-portrait reference, recognizable). The COMMON block of `gen_illustration.py` already bakes this in; the frame just needs to specify where he's standing (e.g. "standing on the hill to the right, holding up a map").  See `visual.md` for how it's drawn.

## IX. The Chinese-Text Checkpoint on the Card

The thesis, caption, and three big questions are all Chinese text going onto the card. Read every sentence aloud silently, and rewrite anything that snags, asking "would a native writer put it this way":

- Turn nominalizations back into verbs: "价值的捕获" ("the capture of value") → "谁把钱赚走" ("who takes the money").
- Cut preposition chains: "通过 X 实现 Y" ("achieve Y through X") → put the verb directly with its object.
- Don't use self-congratulatory words like "这一刀" (this cut), "锋利" (sharp, as meta-commentary), "钉死" (nailed down), "砸实" (hammered solid) — if the content is strong enough, the reader can see it without you narrating your own cleverness.
</content>
