---
name: ljg-paper-river
description: "Paper reverse-reading method (论文倒读法): given a paper, recursively traces the prior papers it criticizes and improves on (up to 5 levels), then finds the latest work that follows it, and narrates the problem's evolution forward from the source. Problem-centered, Feynman-style explanation of the problem each paper saw and the innovation in its solution. Use when user shares a paper and wants to understand its intellectual lineage, citation chain, problem evolution, or says '倒读' (reverse-reading), '论文溯源' (paper tracing), '论文脉络' (paper lineage), 'paper river', 'paper connects', 'trace back', '这篇论文的来龙去脉' (the full story behind this paper), '论文演化' (paper evolution). Also trigger when user wants to understand how a research problem evolved across multiple papers."
user_invocable: true
version: "1.0.0"
---

# ljg-paper-connects: Reverse-Reading Method (倒读法)

A paper isn't an island. It stands on the shoulders of those before it, and stands on their scars too. Dig backward to the root, then look forward again — how the problem grew, what each person saw that others didn't, how the solution closed in on the truth step by step.

## Core logic

The most common mistake in reading papers: looking only at the one in front of you, with no idea where it came from. The reverse-reading method flips this — first find who this paper criticizes and improves on, then find who that paper criticizes, recursing five levels deep, digging to the source. Then turn around and read forward from the source.

Read this way, what you come away with isn't the knowledge of one paper — it's an understanding of an entire line of problem evolution.

## Format constraints

### Org-mode syntax

- Bold uses `*bold*` (single asterisk), `**bold**` is forbidden
- Heading levels start at `*`, don't skip levels

### ASCII Art

All diagrams use plain ASCII characters. Allowed: `+ - | / \ > < v ^ * = ~ . : # [ ] ( ) _ , ; ! ' "` and spaces. Unicode drawing characters are forbidden.

### Template authority

The output structure follows `references/template.org`.

### Denote file conventions

- Timestamp: `date +%Y%m%dT%H%M%S`
- Readable time: `date "+%Y-%m-%d %a %H:%M"`
- Filename: `{timestamp}--paper-river-{short title}__paper_river.org`
- Output directory: `~/Documents/notes/`

### Org file header

```
#+title:      paper-river-{short title}
#+date:       [{YYYY-MM-DD Day HH:MM}]
#+filetags:   :paper:river:
#+identifier: {YYYYMMDDTHHMMSS}
#+source:     {URL or source description}
#+authors:    {target paper's authors}
#+venue:      {venue/year published}
```

## Red lines

1. *Problem as the axis* — the whole piece's main thread is "how the problem evolved," not "how the papers are arranged." Papers are supporting cast, the problem is the protagonist
2. *Spoken-word test* — would you tell a friend the history of a field's development this way? If not, change it
3. *Difference as the core* — each paper's explanation is centered on "where it differs from the one before it," not an independent introduction of each paper
4. *Zero jargon* — ground things in plain language first, then mention the technical term in passing
5. *Unbroken causal chain* — from the first paper to the last, the causal chain can't break. The reader should feel "that's why they did it this way"
6. *Honesty* — if you can't find five levels, say how many you found. If the relationship between papers is uncertain, say so. Don't fabricate citation relationships

## Writing principles

1. *Difference-driven narrative* — don't write an independent summary of each paper and stitch them together. Start each paragraph with "what problem did this paper see in the previous one," and let the difference itself drive the narrative forward
2. *Transformation over definition* — when explaining the difference between two approaches, morph approach A continuously into approach B. "If you remove X and add Y, you get Z" — is ten times more powerful than "the difference between Z and X is..."
3. *Reasoning made visible* — before each solution appears, first let the reader feel the pressure of "this can't go on." Simulate the process of discovery, don't just report the discovered result
4. *One picture beats a thousand words* — draw a lineage map before the evolution narrative, and a compressed overview map after it. Let the reader get the big picture first before diving into details, then return to the big picture after the details

## Execution

### 1. Get the target paper

- arxiv URL → WebFetch
- PDF → Read (mind the pages parameter limit)
- Paper name → WebSearch to find the full text

Make sure you get: title, authors, abstract, introduction (especially the critique of prior work in the related work / introduction sections).

### 2. Extract the critique-chain clues

Read the target paper's introduction and related work sections carefully. Find:

- Where it explicitly says "prior method X has problem Y"
- What paper(s) it claims to improve on
- Who the baselines it compares against are

From this, lock onto the *core papers being criticized/improved on* (usually 1-3, pick the most direct line).

### 3. Recursive tracing (deep research)

For the core prior papers found in step 2, repeat the same process: who are they criticizing? Who are they improving on?

Recursion rules:
- Recurse at most 5 levels (to the 5th level, or to the field's foundational paper, whichever comes first)
- At each level, chase only *the line most relevant to the problem*, don't branch out
- If a level has no clear criticized target, stop there

Use the Research skill (deep research mode) to get key info on each level's paper. Get at least this for every paper: title, authors, year, core problem, core solution, what it criticizes about prior work.

### 4. Extend to the frontier

Reverse direction: after the target paper, is there new work criticizing/improving on it?

Also use the Research skill to search for:
- Later work that cites the target paper
- The latest progress on the same problem

Find the 1-3 most relevant follow-up papers, and get the same info for them.

### 5. Build the evolution line

Organize the results of steps 3 and 4 into a timeline:

```
[oldest] Paper_0 -> Paper_1 -> ... -> [target paper] -> [follow-up papers]
```

Label each arrow: what problem did the latter see in the former.

### 6. Forward Feynman narrative

Starting from the oldest paper, narrate forward. The key: don't introduce each paper independently one by one — string them together using problem evolution as the thread.

Cover three things for each paper (centered on the difference):
1. What specific problem it saw in the prior approach (illustrate with an example or scenario)
2. The core idea behind its solution (explain with an analogy)
3. What new problem this solution left behind (transition naturally to the next paper)

### 7. Draw diagrams

Two diagrams:
- *Lineage map*: placed before the evolution narrative, showing the citation/critique relationships between papers
- *Problem-solution overview*: placed after the narrative, compressing the entire line into one screen. Let someone glance at it and know how this line grew

### 8. Distill the insight

After reading through the whole line, answer:
- What's actually changing beneath this evolution line? (Not the surface-level technical iteration, but the deeper shift in understanding)
- Where is the next step most likely to go?

### 9. Pass the red lines + generate the file

Go through the red-line checklist item by item. Additionally check:
- Is the causal chain coherent — read all the "what problem it saw" statements strung together, does the logic hold up
- Is the difference highlighted — is each paper's focus really on "what's different from before"

Read `references/template.org`, and write to `~/Documents/notes/` following the Denote convention.

## Acceptance criteria

- *Problem is the protagonist*: what you remember after reading is "how the problem evolved," not "which papers exist"
- *Unbroken causality*: from the first paper to the last, every turn has a "that's why"
- *Clear differences*: each paper's unique contribution can be stated in one sentence
- *A layperson can follow*: a smart person unfamiliar with the field can retell this evolution line after reading
- *Both diagrams stand alone*: without reading the body text, the diagrams alone convey the gist
- *Honestly labeled*: clearly marked which citation relationships are confirmed and which are inferred
</content>
