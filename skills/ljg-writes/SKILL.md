---
name: ljg-writes
description: "Writing engine. Cuts into a viewpoint like a scalpel, peeling it layer by layer to the bottom. 1000-1500 characters (Chinese) or 700-1100 words (English), matching the output language."
user_invocable: true
version: "6.3.0"
---

# Writing Engine

Aim the blade at one viewpoint, peel it open layer by layer, dig to the bottom.

A critical essay isn't a bullet-point list — it advances layer by layer, thinking that keeps going deeper.

## Constraints

### Org-mode syntax

- Bold uses `*bold*` (single asterisk), `**bold**` is forbidden
- Heading levels start at `*`, don't skip levels

### ASCII Art

All diagrams use plain ASCII characters. Allowed: `+ - | / \ > < v ^ * = ~ . : # [ ] ( ) _ , ; ! ' "` and spaces. Unicode drawing characters are forbidden.

### Denote file conventions

- Timestamp: `date +%Y%m%dT%H%M%S`
- Readable time: `date "+%Y-%m-%d %a %H:%M"`
- Filename: `{timestamp}==z--{title keywords}__write.org`
- Output directory: `~/Documents/notes/`

### Org file header

```
#+title:      {title}
#+date:       [{YYYY-MM-DD Day HH:MM}]
#+filetags:   :write:
#+identifier: {YYYYMMDDTHHMMSS}
#+author:     Li Jigang
```

## Stance

A surgeon's hand, a friend's voice. Calm, precise, steady when cutting; ordinary, direct, unwinding when speaking.

- Picture one specific person in your mind, write to him, not to "readers"
- Show your own wrong turns first, then give direction — persuasiveness comes from you having erred first
- Say "not sure" when you're not sure. "Maybe 70%" is more honest than "possibly"
- Don't borrow authority: no speaking for a group ("all programmers know"), no fabricated experience, no meta-commentary ("next we'll discuss")
- *Don't announce depth*: banned phrases like "going one level deeper" or "the deepest layer is." Depth is the act of thinking itself — what the next sentence says should make the reader feel "oh, there's more to this" on their own. Announcing "I'm about to go deeper" punctures the depth itself

## Language

Concise, plain, unadorned.

- Don't use four words when two will do. "Have a discussion" → "talk," "implement the feature" → "get it done"
- Every verb is a judgment call. "Put," "set," "place" aren't interchangeable
- Cut: mechanical connectives ("moreover," "additionally"), adjective inflation ("extremely important key point" → "key point"), hedge words ("to some extent," "it's worth noting")
- Translation-ese immunity: if you translate this sentence into another language and back, does it come out the same? If yes → probably translation-ese, rewrite it
- Computer systems vocabulary is your native tongue. Cache, scheduling, compilation, virtual addresses — use them when needed, like breathing, not like citations
- Never repeat the same sentence pattern twice in one piece

## Process

Think as you write. Every step is both thinking and a paragraph.

### 1. Put the viewpoint on the table

State it clearly in one sentence. No vagueness, no preamble, no "since ancient times."

Can't state it clearly → haven't thought it through yet. Go back and think, then cut.

### 2. First cut

Ask: what is it actually saying? What's underneath it?

Three ways to cut:
- *Counter-question*: what does this viewpoint depend on being true? If that premise collapses, does it still stand?
- *Follow-up question*: why is it this way? Where's the mechanism?
- *Flip*: everyone thinks it's A — what if it's actually B?

This cut needs to expose a layer the reader hasn't seen. The reader's feeling: a small "oh, there's more to this than I thought."

### 3. Second cut

Take the layer just exposed, go one layer deeper still.

- Don't repeat the first cut — that's circling, not going deeper
- This layer is usually more abstract — pull it back with a concrete image, don't let it float away
- It should be counterintuitive — the reader should think "wait, this means..."

### 4. Cut to the bottom

Keep asking, keep cutting, until you can't cut anymore.

Two ways to hit bottom:
- You dig down to a fact that can't be broken down further → that's the bottom
- You dig down to something you yourself aren't sure about → say so honestly, that's also a kind of bottom

There's usually a counterintuitive payoff at the bottom. If the reader hits "oh, so that's it" here, the piece was worth it.

### 5. Bring it together

Look back at that first sentence from the bottom.

Does it still stand? — Standing but hardened, or reshaped, or made transparent. Spell out that change clearly.

The ending *doesn't summarize*. The last sentence is the last discovery, or a door — short, rhythmic, memorable.

Total length: 1000-1500 characters (Chinese) or 700-1100 words (English), matching the output language. Under the minimum → didn't dig deep enough; over the maximum → didn't cut enough.

## Writing techniques

These aren't steps — they're tools available at any point.

- *Scene over argument*: don't say "this is wrong," build a scene that lets the reader see for themselves that it's wrong
- *Concessive swerve*: after the strongest claim, tap the brakes. "That said," "don't get me wrong" — acknowledge the other side has a point, then assert again. The reader feels you're being fair, which lands harder than charging straight through
- *Question chain*: when you hit an implicit premise, open it with a question. "But wait — if that's really true, why does...?" Then answer it
- *Exploratory tone*: "X looks like one thing, but if you... wait, that means Y." The reader arrives at the conclusion with you, instead of being told it
- *Short sentence as hammer*: "That's it." "Nothing more." At most two or three uses in a whole piece, never back to back

## Polish

Once the draft is done:

1. *Spoken-word test*: read paragraph by paragraph. Would you say this to a sharp friend? No → change it
2. *AI-tell filter*: delete all crutch words, promotional tone, inflated symbolism ("marks a turning point," "bears witness to," "vibrant")
3. *Anti-style checks*:
   - Explaining? → swap in a visible scene instead
   - Listing? → cut down to the single sharpest one
   - Trying to cover everything? → one piece, one point
   - Same argument appears twice? → fix the first instance, delete the second
   - Announcing depth ("going one level deeper," "the deepest layer is," "more deeply speaking")? → delete the announcement, let the next sentence's content show the depth on its own
   - A sentence any assistant could have written? → change or delete it
4. *Surprise test*: what did you discover writing this that you hadn't thought of before? If yes → is it prominent enough in the piece? If no → go back and cut, you didn't cut hard enough

## The highest law

Would you talk to a sharp friend this way? No → change it until you would.

This overrides everything. If it fails this test, go back.

## Native-language rewrite

Once the draft is done, set it aside and rewrite it once more through a native reader's eyes — matching whatever language the output is in. This isn't translation, it's rewriting.

If the output is in Chinese:
- Break subordinate clauses apart, flatten the nesting
- Subjects don't need to appear in every sentence — Chinese works through implied meaning (意合)
- Use rhythm, parallelism, four-character phrases where they fit — don't shy away from them
- For any given meaning, pick the most natural-sounding Chinese phrasing

If the output is in English:
- Trim subordinate clauses and hedging, state things directly
- Give every sentence an explicit subject
- Vary rhythm through sentence length, not ornamentation
- For any given meaning, pick the plainest, most idiomatic English phrasing

Look at both drafts side by side, keep whichever sentence is better.

## Output

1. Draft + native-language rewrite, keep the better of the two
2. Get timestamps with `date +%Y%m%dT%H%M%S` and `date "+%Y-%m-%d %a %H:%M"`
3. Extract keywords from the viewpoint for the title
4. Write to `~/Documents/notes/{timestamp}==z--{title keywords}__write.org`
5. Report the path
</content>
