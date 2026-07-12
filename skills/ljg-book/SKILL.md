---
name: ljg-book
description: "Book reader that reconstructs a book as x -> f -> f(x): the problem it addresses, the author's central answer, and how that answer changes judgment or action. USE WHEN the user gives a book title, PDF, excerpt, or asks '拆书' (break down a book), '分析这本书' (analyze this book), '这本书在讲什么' (what is this book about), '压缩一本书' (compress a book), or 'book'. Defaults to a saved Markdown note. NOT FOR chapter summaries, framework audits, papers, single-idea deep dives, or field ranking."
user_invocable: true
---

# ljg-book: Reading a Book as x → f → f(x)

Restore a book to one complete logical chain:

- **x**: what problem the author is actually working on.
- **f**: the author's central answer. It may be a concept, a distinction, a framework, a method, a model, or a narrative perspective.
- **f(x)**: putting that answer back into the problem — how we should now re-understand, judge, or act.

The goal is not to compress the book into a platitude, nor to run a framework audit on the author. After reading the note, you should know both "what this book is saying" and "how it changed the way I respond to this problem."

## Workflow Routing

| Input | Must read | Output |
|---|---|---|
| Book title | After finding reliable material, read `ReadingGuide.md` | Save a Markdown note |
| PDF, full text, sample chapter, old notes | Read the material first, then `ReadingGuide.md` | Save a Markdown note |
| User explicitly wants a verbal explanation only | `ReadingGuide.md` | No file; explain along the same path |

Read `references/template.md` when writing the note file. Save to `"/Users/jjin/Documents/Obsidian Vault/Book Notes/"` (quote the path — it has spaces; create the folder if it doesn't exist).

Filenames follow the Denote pattern: `{YYYYMMDDTHHMMSS}--拆书-{book title}__book.md`; generate the timestamp with `date +%Y%m%dT%H%M%S`.

## Completion Target

Open with three ultra-brief conclusion lines:

```markdown
- **x**: {what problem the author is addressing}
- **f**: {the central answer the author uses to handle x}
- **f(x)**: {having accepted f, how to respond to x}
```

The three lines must read as one continuous thought: because x exists, the author proposes f; applying f to x yields f(x). If the three lines are merely three related observations, the logical chain hasn't connected yet.

The body then unfolds this chain. Typically 900-1600 Chinese characters (or roughly 600-1100 English words, matching the output language), excluding the file header and the source calibration; short books can run shorter, and complex books exceed the maximum only when the logic truly requires it.

## ASCII Chart

When f contains clear steps, causation, loops, hierarchy, or opposition, and a diagram is clearer than a paragraph, draw one ASCII chart.

- The chart must serve `x → f → f(x)`, not decorate.
- Put it in a fenced code block so it renders intact in Obsidian.
- No wider than 80 characters.
- One chart is enough; if no relation fits, draw none.

Minimal form:

```
x: the original problem
        |
        v
f: the author's concept / framework / method
        | apply
        v
f(x): new understanding / judgment / action
```

## Gotchas

- **x is not a topic.** "Technology and the future" is not an x; "does technological evolution have a direction of its own" is.
- **f is not a label.** Writing only "systems thinking," "existentialism," or "antifragility" is no answer; say how it handles x.
- **f is not a contribution type either.** Don't output reviewer judgments like sources, load-bearing status, originality, contribution type, or move type, unless the user explicitly asks.
- **Don't jump from x straight to advice.** Without f, the note degenerates into generic life lessons.
- **Don't write f as a table of contents.** It may have 2 to 4 necessary parts, but they must jointly complete one single answer.
- **f(x) is not a slogan.** Spell out what exactly changes — in the prior understanding, the order of judgment, or the action — once f is accepted.
- **Literary works have an f too.** It may not be a methodology but an image, a relationship between characters, or a narrative movement that makes some problem visible anew; don't force it into an efficiency tool.
- **Research stays in the background.** The author's biography, intellectual lineage, and external debates serve only to prevent misreading; the body keeps only what's truly needed to understand x, f, and f(x).
- **The chart is not mandatory.** Draw only when the relations are hard to state in short sentences.
- **Thin material, fewer claims.** With only a table of contents, interviews, or reviews, mark the note "initial breakdown" (初拆); with even thinner evidence, mark it "hypothesis version" (假设版).

## Quick Reference

1. Narrow the topic into one concrete x.
2. Find the main f the author proposes for x, rather than listing every concept in the book.
3. Use 2 to 4 in-book anchors to show how f holds and how it runs.
4. Compute f(x): if f is truly accepted, what changes when facing x.
5. Show the relations with one ASCII chart if necessary.
6. Delete anything that cannot plug into this logical chain.

The top-level sections are fixed:

1. `# x: What problem the author is addressing`
2. `# f: How the author answers`
3. `# f(x): How to respond to the world`
4. `# Source calibration`

## Examples

**Analytical book: *The Psychology of Judgment and Decision Making* (《决策与判断》)**

- **x**: Why do people make opposite decisions when the same choice is phrased differently?
- **f**: Prospect theory explains the judgment shift through reference points, loss aversion, and nonlinear probability weighting.
- **f(x)**: For important choices, first rephrase the gains and losses; if the answer changes with the phrasing, postpone the decision.

**Literary work: *Siddhartha* (《悉达多》)**

- **x**: Can wisdom be passed from one person to another the way knowledge can?
- **f**: Doctrine can point the way, but wisdom must grow out of lived experience, listening, and the integration of contradictory experience; the river is the concentrated image of this answer.
- **f(x)**: When seeking answers from others, first distinguish whether what you lack is teachable knowledge or experience you haven't yet digested yourself.

## Completion

After generating, read the note back and confirm:

- The file is actually saved and the frontmatter is complete.
- The opening has exactly the three `x / f / f(x)` lines.
- x is a problem, f is an answer, f(x) is the result of applying it; the three are not a parallel summary.
- All four body sections are present, and each one extends the same logical chain.
- The body has at least 2 in-book anchors and no chapter-by-chapter summaries following the table of contents.
- f explains the internal mechanism and hasn't degenerated into a term or a list of concepts.
- f(x) concretely states the change in understanding, judgment, or action.
- The body contains no reviewer information such as sources, load-bearing status, contribution type, or move type.
- The ASCII chart appears only where it genuinely aids understanding, with valid format and width.
- Source calibration lists only necessary sources and hasn't grown into a research survey.
- Reading just the three opening lines answers: "What is this book asking, how does it answer, and what do I do once it's answered?"
