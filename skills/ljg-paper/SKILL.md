---
name: ljg-paper
description: "Paper reader for non-academics that reconstructs one paper as x -> f -> f(x): the research problem, the paper's method or explanatory move, and the evidence-bounded result plus judgment update. USE WHEN the user shares an arXiv link OR paper URL OR PDF OR local paper file OR paper title, or asks to read, explain, analyze, or understand a paper. Defaults to a saved Markdown note. NOT FOR experiment reproduction, formal peer review, exhaustive benchmark tables, or literature surveys."
---

# ljg-paper: Reading a Paper as x → f → f(x)

Restore a paper to one complete research logic chain:

- **x**: the specific problem the author is solving, and why the old approach falls short here.
- **f**: the method, mechanism, measurement, or theoretical relation the author proposes to handle x.
- **f(x)**: what result comes from applying f to x; how the evidence lets us update judgment or action.

Written for someone unfamiliar with the field but willing to think. After reading, they should know both "what the paper did" and "why it might work, and how far the result actually reaches."

## Workflow Routing

| Input | Must read | Output |
|---|---|---|
| arXiv, PDF, paper URL, local paper | `ReadingGuide.md` | Save a Markdown note |
| Only a paper title | After finding reliable material, read `ReadingGuide.md` | Save a Markdown note |
| User explicitly wants a verbal explanation only | `ReadingGuide.md` | No file; explain along the same path |

Read `references/template.md` when writing the note file. Save to `"/Users/jjin/Documents/Obsidian Vault/ResearchPaper Notes/"` (quote the path — it has spaces; create the folder if it doesn't exist).

Filenames follow the Denote pattern: `{YYYYMMDDTHHMMSS}--paper-{method name or paper keywords}__paper.md`; generate the timestamp with `date +%Y%m%dT%H%M%S`.

## Completion Target

Open with three ultra-brief conclusion lines:

```markdown
- **x**: {what research problem forces this paper to exist}
- **f**: {the core method, mechanism, or new ruler the author proposes}
- **f(x)**: {the key result + how it should change judgment}
```

The three lines must read as one continuous thought: because the old approach fails at x, the author proposes f; f acts on x under the paper's conditions, producing f(x). If f(x) carries no evidence bound, it's merely inspiration; without a judgment update, it's merely an experiment abstract.

The body typically runs 1000-1800 Chinese characters (or roughly 650-1200 English words, matching the output language), excluding the file header and source calibration. Complex mechanisms can run slightly longer, but don't restate the paper in Abstract / Method / Experiments order.

## ASCII Chart

Method structure and approach comparisons go first into one ASCII chart. The chart's job isn't to display every experimental number — it's to let a layperson see at a glance exactly which step the paper changed.

Pick one main chart type based on the paper:

- **Method flow**: which key steps the input passes through, and where f changes the old pipeline.
- **Approach comparison**: what information and actions the baseline and f each use, and what failure each solves.
- **Causal mechanism**: how variables interact, and why the result changes.
- **Measurement framework**: what the old ruler sees and misses, and what dimension the new ruler adds.

Rules:

- Put it in a fenced code block so it renders intact in Obsidian.
- No wider than 80 characters.
- Usually just one chart; don't split method and comparison into two charts if one will hold both.
- Keep at most 2 to 3 baselines that genuinely explain f.
- Keep only the comparison dimensions that explain "why f differs" — don't copy the full benchmark table.
- Pure theory or explanatory papers without a natural fit can skip the chart.

## Gotchas

- **x is not field background.** "Large models are expensive to run" is too broad; "the model already answers correctly but can't tell when to stop" is a researchable problem.
- **f is not the paper's name or model name.** State which information, step, assumption, or measurement dimension the author changed, and why it might address x.
- **f(x) has two layers.** Write the in-paper result first, then the judgment update it brings; don't jump from the method straight to a life lesson.
- **Results must hang off x.** Keep a number only when it shows how much the old problem improved; usually 1 to 3 key numbers are enough.
- **Comparison is not a leaderboard.** The ASCII chart shows structural differences, not the full set of metrics and baselines.
- **A benchmark contribution is not a scientific discovery.** A new benchmark makes a problem measurable; that's not the same as explaining reality.
- **A resource paper may have no new mechanism.** It may just push cost, scale, or accessibility one step forward — write that plainly.
- **Evidence and scope stay attached to the claim.** State which tasks, data, models, or assumptions the result holds under, rather than adding a separate disclaimer chapter.
- **Don't write a peer-review report.** No strong-accept/reject verdicts, no extended writing-quality critique, author-motive speculation, or full related-work survey, unless the user explicitly asks.
- **Limited idea content is allowed.** If it's an engineering increment, say so; when there's no direct action implication, f(x) can simply update one research judgment.

## Quick Reference

1. Judge the paper's main type: explanatory, method, measurement, resource, or theory.
2. Narrow the field topic into one concrete x, and point out where the old approach fails.
3. Write f as a mechanism that runs, not a method name.
4. Choose a method-flow or approach-comparison ASCII chart to show where f changed things.
5. Compute f(x) from 2 to 4 key pieces of evidence, marking the conditions under which it applies.
6. Derive one judgment or action from f(x); don't invent a separate list of applications.

The top-level sections are fixed:

1. `# x: What problem the paper is solving`
2. `# f: What solution the author proposes`
3. `# f(x): What judgment the result changes`
4. `# Source calibration`

## Examples

**Method paper**

- **x**: The model already answers correctly, but often wastes reasoning tokens because it doesn't know when to stop.
- **f**: Train a residual-reasoning-value signal that judges, at each step, whether continued reasoning is still worth it.
- **f(x)**: If accuracy holds while reasoning length shrinks in testing, the system's missing piece may not be stronger reasoning but closing-out feedback.

```
Old approach: problem -> keep reasoning -> fixed budget exhausted -> answer

New approach: problem -> reason one step -> estimate residual value
                                              |- high: continue
                                              |- low: stop -> answer
```

**Evaluation paper**

- **x**: Topic similarity can find related papers, but can't tell whether a proposal inherits and modifies a prior work's core mechanism.
- **f**: Add a mechanism-lineage dimension that separately checks inheritance and substantive change.
- **f(x)**: When judging a research idea, ask not just "is the topic related" but also "which mechanism line did it pick up and change."

## Completion

After generating, read the note back and confirm:

- The file is actually saved and the frontmatter is complete.
- The opening has exactly the three `x / f / f(x)` lines.
- x is the research problem and the old approach's gap, f is a mechanism that runs, f(x) is the evidence-bound result and judgment update.
- All four body sections are present and jointly complete the same research logic.
- The f section lets a layperson see why the method might work, without degenerating into a component list.
- f(x) has at least 2 key pieces of evidence or one sufficiently strong core result, with applicable conditions stated.
- Where the method or comparison suits visualization, there's one ASCII chart with valid width and fence formatting.
- The chart doesn't copy the full benchmark table or cram in irrelevant baselines.
- The main text has no LaTeX formulas, formal peer-review verdicts, or chapter-by-chapter paper abstracts.
- Reading just the three opening lines answers: "What's the problem, how does the paper solve it, and what should the result make me rethink?"
</content>
