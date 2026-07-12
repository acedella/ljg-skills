# Reading Guide: Building a Paper's x → f → f(x)

This guide rewrites a paper from academic writing order into research logic a layperson can follow. Read the full material first, connect x, f, and f(x) internally, then start writing.

## 1. Judge the material and paper type first

Prefer the original PDF, HTML full text, the official paper page, or an author's version. With only a title, find reliable original text first; if you hit a paywall, turn to arXiv, the author's homepage, or an institutional repository — don't patch the body from a secondhand abstract.

Grade by material:

- **Full read (完整解读)**: full text plus key figures, method, and experiments.
- **Initial read (初读)**: only an abstract, extended abstract, talk, or partial body text.
- **Hypothesis version (假设版)**: material is very thin; only a tentative understanding can be offered.

Then judge one main type:

| Type | What f usually is | What f(x) mainly checks |
|---|---|---|
| Explanatory / discovery | A new relation or causal mechanism | Whether the phenomenon is better explained |
| Method / intervention | An algorithm, pipeline, or training signal | Whether the goal improves, whether the cost changes |
| Measurement / evaluation | A new metric, task, or judging dimension | Whether the old ruler's blind spot is exposed |
| Resource / system | Data, tools, or infrastructure | Whether something previously infeasible becomes feasible |
| Survey / theory | A framework or formal relation organizing facts | Whether scattered facts gain a unified explanation |

Type limits the weight of the claim. An evaluation paper usually changes "how we measure"; a resource paper usually changes "what becomes doable" — neither should be automatically elevated into how the world works.

## 2. Define x: the research problem and the old approach's gap

x is not an introduction to the field's importance — it's a problem where the old method fails under specific conditions.

Write out four things:

- **Object**: who or what system runs into the problem.
- **Goal**: what it originally set out to accomplish.
- **Old path**: how the baseline or usual practice handles it.
- **Gap**: where it fails, in an observable scenario.

Find one concrete anchor that runs through the whole piece — for example: the model's answer is already correct, yet it keeps generating three times the necessary length of reasoning; or a proposal's topic looks close to a prior paper's, yet it inherits the wrong core mechanism from it.

If the paper is first-of-its-kind work with no obvious baseline, don't invent an opponent — explain plainly why this was previously unmeasurable, infeasible, or simply unaddressed.

## 3. Distill f: which step the paper changes

f must be one sentence that runs as a mechanism:

> The old method does B based on A; the paper additionally uses C, turning B into D, and this may relieve x.

f differs by paper type:

- Method paper: input, key intermediate signal, decision rule, output.
- Explanatory paper: the relation between variables, the direction of effect, the mechanism producing the phenomenon.
- Evaluation paper: what's being measured, the old metric's blind spot, the added dimension, the resulting verdict.
- Resource paper: the prior bottleneck, how the resource is provided, how the feasibility boundary moves.
- Theory paper: the core object, the constraints, the derived relation — translate formulas into natural language.

Keep only 2 to 4 irreducible parts. Having many components without explaining how they relate still doesn't count as understanding the method.

## 4. Present the method or comparison with an ASCII chart

First decide what the reader most needs to see clearly:

### Method flow

Suited to algorithms, systems, training pipelines, and intervention papers:

```
input --> old pipeline --> failure point

input --> new signal --> new decision --> output
```

### Approach comparison

Suited to papers whose key contribution is "swapped which information or judging standard is used":

```
                 info used       key action       main blind spot
old approach A   ...             ...              ...
old approach B   ...             ...              ...
paper's f        ...             ...              ...
```

### Causal or measurement relation

Suited to explanatory, theory, and benchmark papers:

```
old ruler: visible signal -----------------> conclusion
                                |- misses: mechanism relation

new ruler: visible signal + mechanism relation --> new conclusion
```

Usually pick just one per paper. Chart width no more than 80 characters; at most 2 to 3 baselines; compare only the dimensions that explain f. After the chart, add one short paragraph stating the takeaway — don't restate it line by line.

## 5. Compute f(x): result and evidence boundary

Put the paper's f back into x and answer: did the problem actually improve, and on what basis?

Choose 2 to 4 key pieces of evidence:

- The main result that best hangs off x.
- An ablation or control that shows the mechanism genuinely works.
- A result that exposes cost, failure conditions, or scope boundary.
- If needed, the single most explanatory counter-intuitive finding.

Usually keep 1 to 3 numbers. Don't just write "improved significantly" — translate it into what judgment it changes. Don't extrapolate an in-benchmark result straight to the real world.

Calibrate the wording by strength:

- Multiple models, multiple tasks, with reasonable counter-example controls: "robustly shows."
- Narrow scope but a clear phenomenon: "shows, under these conditions."
- A single-point result or an early observation: "suggests," "worth questioning."
- A measurement or resource contribution: "makes ... measurable," "makes ... feasible."

## 6. Derive a judgment update from the result

f(x)'s final layer is what the reader should update their judgment on — not an extra list of applications.

It can land on:

- Next time evaluating a similar method, check which mechanism or assumption first.
- Next time facing a similar failure, first diagnose whether it's a capability gap or a feedback gap.
- When designing a system, adjust one decision order or measurement dimension.
- Don't update any action for now — just lower or raise confidence in a claim.

If the judgment still reads like a universally true statement after removing f, it's disconnected from the paper. When there's no direct use, write honestly that it only updates a research judgment.

## 7. Write the body

The body typically runs 1000-1800 Chinese characters (or roughly 650-1200 English words, matching the output language), excluding the file header and source calibration. Write in understanding order, not in the paper's chapter order.

### x: What problem the paper is solving

Open with a concrete failure and explain the old approach and its gap. Stop once the reader starts expecting the new solution.

### f: What solution the author proposes

State f in one plain sentence first, then explain the minimal mechanism. Put the method-flow or baseline-comparison ASCII chart in this section; after the chart, point out the key difference.

### f(x): What judgment the result changes

Write the in-paper result and its applicable conditions first, then the judgment update directly derived from these results. Keep evidence attached to the claim it supports.

### Source calibration

List 2 to 5 items: the paper's own text, code or data (if used), necessary external calibration, and material grade. Each entry states only the source and what it supports — don't pad this with a literature review.

## 8. Output format

Save to:

`"/Users/jjin/Documents/Obsidian Vault/ResearchPaper Notes/{timestamp}--paper-{method name or paper keywords}__paper.md"` (quote the path — it has spaces; create the folder if it doesn't exist)

Timestamps:

- `date +%Y%m%dT%H%M%S`
- `date "+%Y-%m-%d %a %H:%M"`

File header:

```markdown
---
title:      "{One-sentence judgment call, preferring f(x)'s judgment update}"
subtitle:   "{The paper's original title; add an explanation first if needed}"
date:       [{YYYY-MM-DD Day HH:MM}]
tags:       [paper]
identifier: {YYYYMMDDTHHMMSS}
source:     {Original URL or source description}
authors:    {Author list}
venue:      {Venue/year}
---
```

## 9. Style

- Use everyday language; for a term, say it in plain words first, then give the original term in parentheses.
- Don't open with academic boilerplate; don't write "this paper proposes" or "experiments show it outperforms the baseline."
- No LaTeX in the main text; translate necessary formulas into variable relations.
- Don't do formal peer review; don't give accept/reject verdicts.
- Don't write meta-analysis labels like source, load-bearing status, contribution type, or move type.
- The title is a 6-18 Chinese-character judgment (or a comparably short judgment in the output language, roughly 4-12 English words), not a translation of the paper's title.

## 10. Pre-submission check

Complete these in order:

> This paper is trying to solve ______.

> It turns the old method's ______ into ______, because ______.

> Under ______ conditions, the results show ______, so I'll update my judgment to ______.

If the second sentence is left with only a method name, f hasn't been unfolded yet; if the third sentence has no conditions and no evidence, f(x) is over-extrapolating; if the third sentence's judgment still holds after removing the paper, it hasn't truly been taken away from this paper yet.
</content>
