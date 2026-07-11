# Extract Workflow

Extract a piece of information into a Q-A chain.

## Voice Notification

```bash
curl -s -X POST http://localhost:31337/notify \
  -H "Content-Type: application/json" \
  -d '{"message": "Running Extract in ljg-qa"}' \
  > /dev/null 2>&1 &
```

Output text: `Running **Extract** in **ljg-qa**...`

## Step 1: Retrieve the Content

Follow by input type:

| Input | Tool | Note |
|------|------|------|
| URL (regular webpage) | WebFetch | Pages requiring login go through markdown-proxy |
| arxiv link | WebFetch (HTML version) | Get the abstract + method + experiment sections |
| PDF / local file | Read | For large PDFs, use the pages parameter to read in chunks |
| Raw text | Skip | Go straight to Step 2 |
| Paper/book title | WebSearch | Get the URL, then use WebFetch |

Make sure you've captured: the core thesis / chain of argument / key examples / boundary discussion.

## Step 2: Find the Idea Skeleton

After reading, sit silently for 30 seconds and answer this one question:

> What is this piece's core thesis? How does it hold that thesis up? Which few steps are the key turning points?

Write down:

- *Core thesis*: one sentence
- *3-5 key turning points*: one sentence each

This is the spine of the Q chain. If the spine can't stand, everything below it is floating — go back and reread.

## Step 3: Design the Q Chain

Every key turning point grows one Q or a group of Q's. Each Q must satisfy:

1. Can't be answered with one definition — cuts to the crux
2. The answer can be grounded in the source material
3. Has an inheritance relationship with the previous Q (Q2 naturally arises once Q1 is answered)

For the four Q types (action / contrast / causation / boundary) and their patterns, see `../References/QuestionDesign.md`. A good Q chain mixes at least three types.

*Ordering rule*: order by argumentative dependency, not by chapter order. The original may write background before method for narrative reasons, but the Q chain should go "root question first, then the solution, then the cost."

*Quantity*: 5-10 Q's. Fewer means incomplete coverage; more tires out the reader.

## Step 4: Write the A's

Every A is strictly four parts, none skippable, none reordered:

```
*Conclusion*: (one sentence — quotable out of context)

*Formalization*: (compress the idea into one visualizable line using words + simple symbols — see "How to Write a Formalization" below)

*How you got there*:
- Step 1 (short sentence, exactly one reasoning step)
- Step 2
- Step 3

*Boundary*: (under what conditions this conclusion doesn't hold / what the argument hasn't covered)
```

Hard requirements:

- *Conclusion*: quotable out of context. If the reader sends this sentence to a friend, the friend gets it
- *Formalization*: compress the idea into one visualizable line using words + simple symbols (→ = ≠ + × ⊃ ⊥ etc.) — it's "the geometry of ideas," not "the formalism of math." Let the reader see the correspondence at a glance. Details in the "How to Write a Formalization" section of `../References/QuestionDesign.md`
- *Reasoning steps*: each one takes exactly one step of reasoning; the previous step opens the door to the next
- *Boundary*: write "conditions under which it doesn't hold," not "future work" — the former is honesty, the latter is PR-speak

### How to Write a Formalization (key points)

Four common patterns:

- *Equation*: `generalist = coordinator; specialist = the one doing the work`
- *Contrast*: `old: big model = does everything; new: big model = coordinator`
- *Flow*: `data → tokens → answer = loss + waste`
- *Escalation*: `call → interface → bilingual hotline`

ASCII + natural language only. No LaTeX, no complex math symbols. Fit it on one line.

## Step 5: Check the Q Chain's Sense of Direction

Read through the Q order:

- *Inheritance*: once Q1 is answered, does Q2 arise naturally?
- *Dependency*: if you delete Q3, does Q4 still hold?

If the Q's are parallel (deleting one doesn't affect the others), reorder or merge them. The Q chain is a path, not a checklist.

You can sketch it out (not to go in the org file, just your own scaffolding):

```
Q1 ─┬─→ Q2
    └─→ Q3
Q2 ──→ Q4
Q4 ──→ Q5 (closing counter-question)
```

## Step 6: Cross the Red Lines

Sweep through, one by one:

- [ ] No Q can be waved away with one definition
- [ ] Every A has all four parts (conclusion / formalization / steps / boundary)
- [ ] The formalization sentence shows the relationship at a glance — not a math formula, not a pile of jargon
- [ ] The Q chain has direction (not a parallel list)
- [ ] No "what is X"-type Q's
- [ ] Q sentences ≤ 20 words (scannable in one glance)
- [ ] No academic tone ("it's worth noting that," "in summary," "in the context of...")
- [ ] A doesn't play it safe by stacking jargon — all jargon is translated into concrete actions or objects
- [ ] 5-10 Q's

Fail any of these, go back and fix it.

## Step 7: Write the File

Get the timestamp:

```bash
date +%Y%m%dT%H%M%S         # → identifier
date "+%Y-%m-%d %a %H:%M"   # → date field
```

denote schema filename: `{YYYYMMDDTHHMMSS}--qa-{topic}__qa.org`

- `qa-` prefix: marks the Q-A type (parallel structure to ljg-paper's `paper-` prefix)
- topic: a 5-10 word distillation of the core thesis, punctuation stripped. Prefer the method name / concept name / a key phrase from the source
- `__qa` suffix: keyword tag, for denote search

Output path: `~/Documents/notes/`

After writing, report the path to the user.

## File Structure

```org
#+title:      {one refined sentence of the core idea — 10-25 words}
#+subtitle:   {original title}
#+date:       [{YYYY-MM-DD Day HH:MM}]
#+filetags:   :qa:
#+identifier: {YYYYMMDDTHHMMSS}
#+source:     {URL or source}

* Lead-in

(One paragraph, 3-5 sentences: what this piece is about, why it's worth pulling apart. Ground the reader, don't summarize.)

* Q1: {one sharp question, ≤ 20 words}

  *Conclusion*: ...

  *Formalization*: ... (e.g. `A = B + C` / `old: X → new: Y`)

  *How you got there*:
  - ...
  - ...
  - ...

  *Boundary*: ...

* Q2: ...

  ...

* Closing

(One sentence that pins down the whole Q-A chain: what is the thing the author actually contributed. Not a summary — a naming.)
```

Note:

- Bold uses `*bold*` (org-mode), not `**bold**` (markdown)
- Lists use `- item`, not `* item` (`*` is a heading in org)
- Separate with blank lines or heading levels, not `---`
- Code uses `~code~` or `=code=`, not backticks

## Acceptance Criteria

- *Q cuts to the crux*: no Q can be waved away with one definition
- *A closes off with a formalization*: every A has all four parts — the conclusion sentence is quotable, the formalization shows the relationship at a glance
- *Q chain has direction*: deleting one Q collapses what follows
- *Doesn't restate the original*: it's a skeleton rebuild, not a paraphrase
- *Native-language phrasing*: read every sentence silently and check whether it sounds like a native speaker — driven by verbs, no jargon-stacking, no academic tone
</content>
