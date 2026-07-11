---
name: ljg-paper
description: "Paper reader for non-academics, especially large-model and AI papers. Use when the user shares an arxiv link, paper URL, PDF, local paper file, paper title, or asks in Chinese/English to read, explain, analyze, or understand a research paper. The skill ignores academic completeness and translates the paper into a world-model update: what common belief it argues against, what new claim it makes, why the claim is somewhat credible, how to change one's judgment about large models, where the claim breaks, and what the reader can do with it. NOT FOR reproducing experiments, exhaustive method summaries, formal peer review, benchmark tables, or literature surveys."
---

# ljg-paper: Translating a paper into a world-model update

This isn't a close-reading tool, nor a peer-review tool. It does one thing only: translate a paper about large models into a judgment a non-specialist can walk away with.

The reader doesn't need to know the full experiments, data details, formula derivations, or model architecture diagrams. The reader needs to know: which of my views about large models did this paper shift, by how much, and where should I be careful not to misapply it.

## Workflow Routing

| Input | Approach |
|------|------|
| arxiv / PDF / paper URL / local paper | Read the paper, extracting only the information needed to answer the six questions |
| Only a paper title | Find the paper first, then read it using the same framework |
| User says "explain this paper," "read this paper," "paper," "analyze this one" | By default, generate an org note and save it |
| User just wants a verbal explanation | Don't write a file, explain verbally using the same six sections |

## Gotchas

- Don't mistake "non-specialist" for "add lots of analogies." Use analogies only when they change understanding; decorative analogies slow readers down.
- Don't recount the paper's structure. The order of Abstract / Introduction / Method / Experiments is not the reader's order of understanding.
- Don't force a discussion of the method. The method should appear only when it explains why the new idea might hold.
- Don't force numbers in. Numbers should appear only when they change a credibility judgment; pick at most 1-2 key numbers.
- Don't act like a PhD advisor doing peer review. Here we judge "how much should I believe this," not "should this be accepted."
- Don't dress the paper up as a trend prophecy. Extrapolate if it's warranted, say you can't if it isn't.
- If the paper is mainly an engineering increment with no clear idea, say so directly: "this paper's idea content is limited."

## What the reader should walk away with

Six months later, the reader only needs to be able to recite five sentences:

1. What people used to easily assume.
2. What this paper says is wrong about that assumption.
3. What new judgment it offers.
4. Why I can believe it a little, for now.
5. Going forward, when I look at large models, agents, training, evaluation, or products, which question should I ask more?

These five sentences matter more than "what's the method called" or "which dataset was it tested on."

## Core reading approach

First treat the paper as a piece of intellectual testimony, not a technical manual.

Ask six questions of every paper, and structure the output along these same six sections:

1. *Quick take*: what does this paper actually say.
2. *What it's arguing against*: what old view does the paper aim to overturn, revise, or shake.
3. *Its new idea*: what judgment is the author really betting on.
4. *Why it's somewhat credible*: what phenomenon or verification makes the author's judgment a bit more credible.
5. *How I should update my world model*: going forward, how should my rule for judging large models change.
6. *Boundaries*: where this judgment doesn't apply, or shouldn't be trusted too fully yet.

If you can't write section 2 and section 3, it means you haven't understood the paper yet. Don't paper over this with method names, framework names, or benchmark names.

## Information extraction

Once you have the paper, extract only this:

- Title, authors, source, year.
- The core misconception the author wants to address, from the abstract and introduction.
- The minimal mechanism in the method section sufficient to explain the new idea.
- The evidence in the results section that most changes credibility.
- Limitations, failure cases, applicable conditions.
- Boundaries the paper doesn't state explicitly but can be inferred from the argument.

Don't extract the following, unless they directly affect the new judgment:

- Full experimental setup.
- Dataset details.
- All baselines.
- Full metrics tables.
- Formula derivations.
- Related-work surveys.
- Implementation parameters.

## How to write the six sections

### 1. Quick take

Place it at the front, but write it last.

Use three lines to let the reader decide whether to read the whole thing:

- *One sentence*: what people used to think; what this paper says should change.
- *Most worth remembering*: if the reader can only take away one judgment, which one.
- *Don't misread this yet*: what this paper does NOT prove, or what can't be inferred from it.

The quick take is not a summary. A summary describes what the paper did; the quick take describes what needs to change in the reader's head.

### 2. What it's arguing against

First find the old view. The old view can come from field consensus, product intuition, research habits, or a misconception ordinary people commonly hold.

Phrasing:

- "We tend to assume..."
- "Many practices default to assuming..."
- "When people see a large model fail, they commonly attribute it to..."

Then explain why the paper thinks this old view isn't good enough.

Don't rush to introduce the author's method here. The reader must first know "where the old map is drawn wrong" before caring about the new map.

### 3. Its new idea

Write the judgment the author is really betting on as a plain, human sentence.

Good sentences look like this:

- "The model isn't incapable of reflecting — training just never let it pause at the wrong spots."
- "Agent failure isn't necessarily the model being dumb — it's the system lacking division of labor, memory, and handoff."
- "Long reasoning doesn't necessarily mean thinking better — it might just mean not knowing how to stop."
- "What the evaluation sees isn't the capability itself, but a shadow jointly cast by the question, the prompt, and the scoring method."

Bad sentences look like this:

- "This paper proposes a new framework."
- "The authors designed a new training method."
- "Experiments show the method outperforms baselines."

This section can touch on the method simply, but only as far as needed for the reader to understand the new judgment. Don't unfold the full technical pipeline.

### 4. Why it's somewhat credible

This isn't peer review. Just answer: does this paper make the new judgment more credible than before?

Write it in three layers:

1. *What it saw*: the phenomenon, failure mode, or anomalous result the author caught.
2. *How it confirmed it a bit*: what the minimal verification was — don't unfold the full experimental log.
3. *How much I believe it*: strong / medium / weak, with reasons.

Be disciplined about credibility judgments:

- Strong: the same pattern shows up across multiple models, tasks, or settings, with reasonable counterexample controls.
- Medium: the phenomenon is clear, but the scope is narrow, few models, tasks feel contrived, or the evidence mainly supports a directional sense.
- Weak: more like a suggestive observation, an engineering trick, or a single-point result — worth remembering but not yet a rule.

Write numbers only when they change a judgment. When writing a number, translate it into plain language: does it mean a large gap, a small gap, something that only holds under a certain condition, or something that merely looks impressive.

### 5. How I should update my world model

This is the heart of the whole piece.

Turn the paper into a usable judgment rule going forward. The sentence pattern should be:

- "Next time I see X, I won't rush to judge Y — I'll ask Z first."
- "Next time I evaluate an agent, I won't just ask how strong the model is — I'll also ask whether it has..."
- "Next time I see a benchmark score improve, I'll first check whether it swapped out..."
- "Next time I design my own system, I can treat ... as a controllable variable."

Don't stop at "this makes us rethink things." State clearly which specific view is being updated.

Prioritize placing the paper into one of these large-model master themes:

- *Capability boundaries*: what a model actually can and can't do, where failures come from.
- *Training mechanism*: what training signal grows a capability, what signal locks a capability in place.
- *Reasoning mode*: does the model think while writing, think before writing, or is it forced to lay its thoughts out on tokens.
- *Agent organization*: do failures come from individual intelligence, or from memory, division of labor, oversight, permissions, rollback.
- *Evaluation illusion*: does the benchmark measure capability, or a projection created by the question and the scoring method.
- *Product implications*: what tool, interface, workflow, or automation system does this judgment change.

### 6. Boundaries

Every piece must state the boundaries. Boundaries aren't nitpicking — they're there to keep the reader from overusing the paper.

Answer at least three things:

- Which models, tasks, scales, or scenarios it might hold in.
- What it does NOT prove.
- What ordinary readers are most likely to misread it as.

If the paper gives failure cases, use them. If not, infer the boundaries from the argument's conditions.

The clearer the boundaries are written, the more the reader truly owns the idea.

## Output format

When generating an org-mode note, the section names are fixed:

1. `* Quick take`
2. `* What it's arguing against`
3. `* Its new idea`
4. `* Why it's somewhat credible`
5. `* How I should update my world model`
6. `* Boundaries`

Org syntax:

- Bold uses `*bold*` single asterisk; `**bold**` is forbidden.
- Heading levels start from `*`, don't skip levels.
- Diagrams can use plain ASCII; don't draw complex diagrams just to look nice.
- No LaTeX formulas in the main text. Translate into natural language when necessary.

Structure follows `references/template.org`. Don't reference old paper notes in `~/Documents/notes/` for structure — old files may come from an outdated template.

## File conventions

Write to `~/Documents/notes/`.

Filename:

- Timestamp: `date +%Y%m%dT%H%M%S`
- Human-readable time: `date "+%Y-%m-%d %a %H:%M"`
- Format: `{timestamp}--paper-{short title}__paper.org`
- The short title uses the method name, core concept, or paper keywords, for easy searching.

File header:

```org
#+title:      {One sentence stating the judgment update this paper brings, in the same language as the input}
#+subtitle:   {The paper's original title; if needed, add a sentence of explanation first, then the original title}
#+date:       [{YYYY-MM-DD Day HH:MM}]
#+filetags:   :paper:
#+identifier: {YYYYMMDDTHHMMSS}
#+source:     {URL or source description}
#+authors:    {author list}
#+venue:      {venue/year}
```

### title

The title is not a translation of the paper's title, nor a method name. The title should state the judgment the reader can walk away with.

Rules:

- Aim for a concise sentence (roughly 6-18 characters if written in Chinese).
- Don't mix in English terms, except product names like GPT or Claude.
- Use a verb or a judgment as the skeleton.
- Don't write "based on," "toward," "a kind of," "framework," "method."
- If the condensed version leaves others unable to guess the direction, fall back on a subtitle to anchor it.

Examples:

| What the paper actually means | Don't write it like this | Write it like this |
|--------------|------------|------------|
| Long reasoning might be wasting tokens | Token efficiency in long-chain reasoning | Thinking well also means knowing when to stop |
| Agent failure comes from organizational problems | An analysis of multi-agent collaboration frameworks | Being smart doesn't mean being able to collaborate |
| Reward locks in already-known paths | The effect of reinforcement learning on exploration | Learning something can become a cage |
| Evaluation creates an illusion of capability | A study of large-model benchmark bias | The score isn't the capability itself |

## Writing red lines

- Don't write "this paper proposes." Write "they set out to prove," "they found," "they're arguing against."
- Don't open with academic-register language. Get into the old view and the new judgment in the first paragraph.
- Don't pile on jargon. The first time a term appears, explain it in plain language, then give the original term in parentheses.
- Don't treat the experimental method as the main thread. The method is evidence's attendant, not the protagonist.
- Don't write for completeness's sake. Readers don't want to reproduce the paper.
- Don't talk up a weak paper into a big idea. If confidence is weak, write it as weak.
- Don't force-elevate an engineering trick into a philosophical insight.
- Don't write empty takeaways like "worth further exploration in the future."

## Self-check

After writing, check item by item:

- All six sections answer one clear question each.
- Sections 2 and 3 aren't papered over with method names.
- Section 4 gives a strong / medium / weak credibility rating.
- Section 5 forms a usable judgment rule going forward.
- Section 6 clearly states what can't be inferred.
- The full text doesn't fully recount the experimental procedure.
- Numbers stay within necessary bounds, and all are translated into meaning.
- The title states the judgment update, not the paper's method.
- Reading only the "Quick take" six months later still brings back why this paper was worth keeping.

## Completion

Read `references/template.org`, write into `~/Documents/notes/` following the template, then read the file back to confirm:

- Frontmatter is complete.
- The six section names are correct.
- No old template sections remain: Questions, Translation, Core Concepts, Insights, PhD-advisor peer review, Verification, Implications.
- No LaTeX formulas in the main text.
