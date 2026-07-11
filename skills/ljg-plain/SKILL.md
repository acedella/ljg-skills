---
name: ljg-plain
description: "Cognitive atom: Plain (白). Rewrites any content so a smart 12-year-old groks it. Structure-free — form follows content. Use when user says '白话说' (say it plainly), '说人话' (talk like a human), '解释一下' (explain it), 'plain', 'grok'."
user_invocable: true
version: "5.0.0"
---

# ljg-plain: Plain (白)

Make people grok it.

Doesn't prescribe how to write. Prescribes how not to write. The floor is locked, the ceiling is open. Different topics have different best forms — analogy, story, Q&A, a chain of escalating examples, one long scene — content decides the form.

## Format constraints

### Org-mode syntax

- Bold uses `*bold*` (single asterisk), `**bold**` is forbidden
- Heading levels start at `*`, don't skip levels

### ASCII Art

All diagrams use plain ASCII characters. Allowed: `+ - | / \ > < v ^ * = ~ . : # [ ] ( ) _ , ; ! ' "` and spaces. Unicode drawing characters are forbidden.

### Denote file conventions

- Timestamp: `date +%Y%m%dT%H%M%S`
- Readable time: `date "+%Y-%m-%d %a %H:%M"`
- Filename: `{timestamp}--plain-{short title}__plain.org`
- Output directory: `~/Documents/notes/`

### Org file header

```
#+title:      plain-{short title}
#+date:       [{YYYY-MM-DD Day HH:MM}]
#+filetags:   :plain:atom:
#+identifier: {YYYYMMDDTHHMMSS}
#+source:     {URL or source description}
```

Report the path after the file is written.

## Red lines (every one must pass, order is priority)

1. *Spoken-word test* — the highest law. Read it out loud: would you talk to a sharp friend this way? No → change it until you would. Connectives aren't the enemy — "but" and "so" are the sound of a thought turning a corner. Only cut mechanical connectives ("moreover," "it's worth noting")
2. *Zero jargon* — a smart 12-year-old should be able to retell it. When a technical term has to appear, ground its meaning in plain language first, then mention the term in passing
3. *Short words first* — don't use four words when two will do. "Conduct an analysis of" → "look at." Big words don't make you sound smart, they just make people tired of reading
4. *One thing per sentence* — every sentence advances exactly one step. Break up long sentences
5. *Concrete* — nouns you can see, verbs with force. "Someone thinks things aren't looking great" → "Zhang San says the project's about to die." Cut adjectives wherever you can
6. *Give the reason up front* — the first sentence should make people want to read the next one. No preamble, no background, no "since ancient times"
7. *No filler* — cut the throat-clearing, the crutch words, the inflated symbolism. Every sentence should be doing work
8. *Trust the reader* — skip the hedging, the justifying, the hand-holding. Saying it once is enough
9. *Honesty* — if you haven't thought it through, say so. "Maybe 70%" is more honest than "possibly"

## Toolbox (optional, don't need to use them all)

Tools you can reach for while writing — none of them mandatory:

- *Analogy* — find an everyday experience with matching structure. A good analogy bears weight (remove it and the piece collapses), has multiple layers (dig one layer deeper and it still holds), and is self-evident (doesn't need the analogy itself explained). When extending the verb to a new object, check whether the verb-object pairing sounds natural in the output language
- *Good question* — find where the reader gets stuck, turn it into a question. A reader who's stuck wants to keep reading
- *The crack* — where does the model/analogy fall short? That point is often the most valuable. Don't announce it, let the reader feel it themselves
- *Image* — a scene you can see with your eyes closed. A forced image is worse than no image
- *Story* — one specific person hits one specific problem. The reader follows along
- *Question chain* — when you hit an implicit premise, open it with a question, then answer it
- *Skeleton diagram* — when a concept involves spatial relationships, embed an ASCII diagram (`#+begin_example` block)

## Execution

### 1. Get the content

URL → WebFetch | text → use directly | file path → Read | concept → explain directly | book/paper title → WebSearch

### 2. Write

Form is free. Pick whichever tool from the toolbox fits this topic best, or pick none — if there's a better way to write it, use that instead.

The output is one continuous piece flowing from the first line to the last. The whole piece has only the file title, no subheadings in the body.

Forbidden:
- Structural labels (`* Analogy` / `* Crack` etc.)
- Meta-commentary pointing at the writing process ("to give an analogy," "next let's discuss")

### 3. Pass the red lines

Go through the red-line checklist item by item. Additionally check:

- Break the formula — negation-style parallelism no more than twice in the whole piece; turn three-part structures into two or four items
- Vary the rhythm — alternate long and short sentences, vary how paragraphs end
- Kill the soundbites — anything that sounds too quotable, rewrite it
- Check for jumps — is every step of logic traceable? If the previous sentence says A and the next jumps to B → add a bridge
- Check for translation feel — does the verb-object pairing sound natural in the output language? If not → swap the verb or the sentence structure

Once done, list the changes made (which sentence triggered what, before → after). This list doesn't go into the file.

### 4. Generate the org file

Get the timestamp per the Denote convention, write the file header + body, save to `~/Documents/notes/`.

## Acceptance criteria

- *Grok*: after reading, can retell the core idea in their own words
- *Zero jargon*: a smart 12-year-old can keep up
- *Memorable*: after reading, something stays in the mind — an image, a question, a turn, anything at all
- *Wants to finish*: no paragraph from start to end that makes you want to skip it
</content>
