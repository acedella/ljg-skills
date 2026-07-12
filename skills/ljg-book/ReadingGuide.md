# Reading Guide: Building x → f → f(x)

The body is not a chapter summary, nor a single plucked life lesson. It must rebuild the book's main logic: what problem the author saw, what answer they gave, and how that answer changes the way we handle the problem.

First work out x, f, and f(x) internally and confirm the three connect — then start writing.

## 1. Look at the material first

When given only a title, first obtain material sufficient to support the whole logical chain:

- Table of contents, introduction, sample chapters, full text, or verifiable in-book excerpts.
- If the bibliographic facts are shaky, check the author's or publisher's page.
- Consult author interviews, reliable reviews, or external baselines only when a factual error would change x or f.

When given a PDF, full text, sample chapter, or old notes, read the source first. Research exists to rule out misreading, not to display effort.

Decide the strength of judgment from the material:

- **Full breakdown (完整拆书)**: full text or sufficient in-book material.
- **Initial breakdown (初拆)**: only a table of contents, introduction, sample chapter, interviews, or reliable reviews.
- **Hypothesis version (假设版)**: very thin material; only clearly-marked tentative understanding is possible.

The material grade goes at the end, under "Source calibration."

## 2. Define x: what problem the author is working on

x must be an answerable question, not a topic name.

Ask in order:

- Which dilemma made the author have to write this book?
- Where does the old understanding break down?
- Which question does the whole book keep returning to?
- Would this sentence still hold for another book in the same genre? If yes, keep narrowing.

A book may hold several problems. Choose the main x that best organizes the whole book; secondary questions enter the body only when they serve the main line.

## 3. Distill f: how the author answers

f is the author's core way of handling x. It may be:

- A concept or a renaming.
- A key distinction.
- A framework or model.
- A method or sequence of operations.
- A causal mechanism.
- A recurring image, character relationship, or narrative movement.

Don't just report the term. Write f as one sentence that runs:

> Facing x, stop understanding it through A; instead, see C by way of B.

f may have 2 to 4 necessary parts, but you must explain how those parts jointly answer x. An irreducible list is not a framework; explaining the relations is what counts.

## 4. Compute f(x): what to do once it's answered

Put f back into x and watch what it changes:

- **Seeing**: which signal was previously ignored, and what becomes visible now.
- **Judging**: what standard was used before, and what standard replaces it.
- **Asking**: what used to be asked first, and what gets asked first now.
- **Acting**: how it was handled before, and which order or move changes.
- **Restraint**: what used to happen automatically, and where one now pauses first.

f(x) is not an appended personal reflection. It must be derived from f. If f(x) still reads like a universally true sentence after f is deleted, it's written too broadly.

Literary works need no forced procedure. f(x) can be a new kind of accommodation, attention, or judgment: the next time the reader meets a certain kind of experience, they no longer rush to name it the way they used to.

## 5. Connect the chain with in-book material

The body usually needs 2 to 4 in-book anchors: scenes, experiments, character passages, cases, distinctions, or metaphors.

Every anchor must do a job:

- Prove that x really is the book's problem.
- Show how one part of f runs.
- Show why f(x) is not advice added out of thin air.

Anchors are not a chapter list. They should point at the same f from different positions.

## 6. Decide whether an ASCII chart is needed

Consider a chart first when these relations appear:

- f has sequential steps.
- Multiple parts have causal or feedback relations.
- The book replaces an old path with a new one.
- A novel completes one shift in understanding through a few key turns.

The chart must make the relations visible faster than prose. Put it in a fenced code block:

```
...
```

No wider than 80 characters, and only one chart. If it merely re-typesets the three summary lines, don't draw it.

## 7. Write the body

The body typically runs 900-1600 Chinese characters (or roughly 600-1100 English words, matching the output language), excluding the file header and source calibration. Short books can be shorter; complex books slightly longer — but don't expand into a full-book survey for completeness' sake.

### x: What problem the author is addressing

Make clear why the problem is real and why the old response falls short. Ground x with one in-book anchor; don't write a sweeping history of the field.

### f: How the author answers

This is the body's center of gravity. Explain f's necessary parts, the relations between the parts, and how the book's material makes it hold. If an ASCII chart clearly improves clarity, place it at the end of this section.

### f(x): How to respond to the world

Put f back into x. State what concretely changes in understanding, judgment, questioning, action, or restraint. Don't invent a separate list of applications; write only consequences directly derived from f.

### Source calibration

List 2 to 5 pieces of material that genuinely support the main line:

- In-book material: full text, sample chapters, table of contents, or introduction.
- Necessary calibration: list author material or external sources only when they truly affect x or f.
- Material grade: full breakdown, initial breakdown, or hypothesis version.

Each entry is just "source + what it supports." Don't pad this section with intellectual history or reviewer judgments.

## 8. Output format

Save to:

`"/Users/jjin/Documents/Obsidian Vault/Book Notes/{timestamp}--拆书-{book title}__book.md"` (quote the path — it has spaces; create the folder if it doesn't exist)

Timestamps:

- `date +%Y%m%dT%H%M%S`
- `date "+%Y-%m-%d %a %H:%M"`

File header:

```markdown
---
title:      "Book breakdown (拆书): {book title}"
subtitle:   "{author} | {one-sentence f}"
date:       [{YYYY-MM-DD Day HH:MM}]
tags:       [book, {domain}]
identifier: {YYYYMMDDTHHMMSS}
---
```

## 9. Style

Concise and direct, like a person talking. Fully unfolding the logic is allowed; stacking terminology to fake depth is not.

Minimize "the author believes," "chapter N points out," "in this sense," "offers a possibility." Write the problem, the answer, and how the answer runs — directly.

Don't use reviewer labels like "sources, load-bearing status, contribution type, move type, originality," unless the user explicitly asks.

Banned legacy pet phrases (and their equivalents in any output language): 骨架抽出来 ("pull out the skeleton"), 精神内核就在你手里 ("the spiritual core is now in your hands"), 走两步 ("take it two steps further"), delta, 钉死 ("nail it down"), 这一刀 ("this cut"), 锋利 ("razor-sharp"), 砸实 ("hammer it solid"), 带走的那一件 ("the one thing you take away").

## 10. Pre-submission check

Complete these in order:

> This book keeps asking ______.

> Its answer is ______.

> Therefore, next time I face this problem, I will ______.

If the third sentence still holds after removing the second, f(x) is too broad. If the first sentence holds for a different book too, x is too wide. If the second sentence is left with only a term, f hasn't been unfolded yet.
