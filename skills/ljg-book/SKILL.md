---
name: ljg-book
description: Break down a book to extract only what the reader can truly take away — what problem the author is addressing, what new insight they offer (a concept, method, framework, or a single sentence), and how to use it the next time this problem comes up. Use when user says '拆书' (break down a book), '分析这本书' (analyze this book), '这本书在讲什么' (what is this book about), '压缩一本书' (compress a book), 'book', or gives a book title for structural analysis. NOT FOR chapter summaries, papers, single-idea deep dives, or field ranking.
user_invocable: true
---

# ljg-book: Book Breakdown (拆书)

Input a book, output one thing the reader can take away.

Don't write a reading-response essay. Don't write a chapter summary. Don't write a review report.

This skill asks only four questions:

1. What problem is the author answering?
2. What new insight did they offer?
3. How do I use it the next time I run into this problem?
4. Where can it NOT be used?

Better to have less, as long as it can be absorbed. Don't scatter the main thread for the sake of coverage.

## Core goal

Six months later, the reader only needs to remember three sentences:

- The problem this book addresses is: ...
- The new thing the author offers is: ...
- Here's how I can use it going forward: ...

If you can't write these three sentences, the breakdown isn't done yet.

## Check the material first

If only given a book title, look up first:

- The author, the publisher's page, the table of contents, the introduction or a sample chapter.
- Author interviews or reliable reviews.
- The common prior answers to this problem.

If given a PDF, the full text, or a sample chapter, read the original first. Unless the user forbids going online, still do a light check on the baseline — don't just trust the strawman the author sets up.

If material is insufficient, downgrade accordingly:

- Full text or enough in-book material: do a complete breakdown.
- Only table of contents, introduction, interviews, reviews: write a "first-pass breakdown," go light on conclusions.
- Very thin material: write a "hypothesis version," don't pretend to have read it.

Don't put the material tier up front to steal the show. Put it at the end, in "Material calibration."

## Body structure

After the file header and before the first heading, put three lines:

```org
- *Problem*: what problem the author is answering
- *New insight*: concept / method / framework / one sentence — pick one
- *Usage*: how I should use it next time I encounter this problem
```

The body has 3 to 5 sections. Headings should grow out of the book itself — don't force a fixed template. Recommended order:

1. Problem: what trouble the author is actually dealing with.
2. New insight: what new thing they offer.
3. Usage: how to use this thing going forward.
4. Boundaries: where it can't be used carelessly.
5. Material calibration: where the evidence comes from.

Diagrams, reference frames, and school-of-thought maps are not required. Only draw one if it makes "problem -> new insight -> usage" clearer. Even then, keep it short.

## 1. Find the problem

A problem is not a topic.

"Technology and the future" is a topic. "Does the evolution of technology have a direction of its own" is a problem.

When finding the problem, ask:

- Why did the author write this book?
- What does he think people originally got wrong?
- What does the reader most likely think before opening the book?
- If this book could only answer one question, what would that sentence be?

The phrasing must be specific. If it would also hold for describing another book in the same field, it's too generic.

## 2. Find the new insight

Pick only one main item for the new insight — don't pile several on.

It can be:

- A *concept*: a term the author coined, or something re-named.
- A *method*: a set of practices, a checklist order, training moves.
- A *framework*: a pair of glasses for looking at the problem.
- A *single sentence*: the most defensible judgment in the whole book.

Use the book's own words. 1-3 terms, checklists, or distinctions unique to this book must show up. Without these words, you're breaking down the field, not the book.

Don't rush to judge whether the author is original. What matters more is: how does this thing change how I see or act going forward?

You can add a one-line note on provenance:

- This is newly coined by the author.
- This is borrowed, but applied to a new place.
- This is an old framework the author turned into a set of moves.
- Evidence is insufficient, don't strongly judge provenance.

But the provenance judgment is just one line — don't let it become the main course.

## 3. Write the usage

This is the most important section.

Translate the new insight into an executable action:

- When facing what problem should I use it?
- Which question do I ask first?
- Which signal do I watch for?
- Which action do I take?
- Which misuse should I avoid?

Write it as a sentence like:

"Next time I run into X, don't jump to Y first; use the author's Z, look at A, then decide B."

The usage must be able to stand apart from the book. It can't just be soft phrases like "we should be more patient" or "think systemically."

## 4. Write the boundaries

Every good insight has a cost. Keep the boundaries section short, but don't skip it.

Ask:

- Under what circumstances would it mislead people?
- What does it miss?
- What conditions does it assume?
- If you follow it, what cost might you pay?

Boundaries aren't nitpicking. Boundaries are what stops a book from being treated as a master key.

## 5. Material calibration

Put a short section at the end. 3 to 6 items.

Each item states the material type and link:

- In-book evidence: full text, sample chapter, table of contents, introduction.
- Author material: interviews, talks, author's homepage, publisher's intro.
- External baseline: prior schools of thought, debates, critiques.
- Real-world material: cases used to verify usage or boundaries.

Material calibration serves the main thread — don't let it overshadow the body.

## Output

Write to:

`~/Documents/notes/{timestamp}--book-breakdown-{book title}__book.org`

Timestamp:

- `date +%Y%m%dT%H%M%S`
- `date "+%Y-%m-%d %a %H:%M"`

File header:

```org
#+TITLE: Book Breakdown: 《{book title}》
#+SUBTITLE: {author} | {one-sentence core view}
#+DATE: [{YYYY-MM-DD Day HH:MM}]
#+FILETAGS: :book:{field}:
#+IDENTIFIER: {YYYYMMDDTHHMMSS}
```

Keep the body at a length that's readable and takeable-away — 3 to 5 sections by default. When the user hasn't asked for a deep breakdown, don't write an encyclopedia.

## Voice

Plain and direct, like someone talking.

Don't flatter the author. Don't disparage the author. Don't defend the author.

Use less:

- "the author believes"
- "the author argues"
- "chapter N points out"
- "in this sense"
- "through the lens of"
- "offers one possibility"

Write more:

- "He changed X into Y."
- "He gave a checklist order."
- "This can be used like this from now on."
- "This is where it backfires."

Banned old catchphrases:

- "pull out the skeleton"
- "the spirit is in your hands"
- "take two steps"
- "delta"
- "nail it down"
- "this cut"
- "sharp"
- "solidly land"
- "the one thing to take away"

Read each paragraph once. If it snags, rewrite the whole sentence. Don't rely on adjectives to manufacture force.

## Pre-delivery self-check

Before delivery, check only these eight items:

1. Are the opening three lines understandable at a glance?
2. Is the problem specific, not a topic?
3. Does the new insight pick only one main item?
4. Do the book's own terms show up?
5. Can the usage stand apart from the book?
6. Are the boundaries clear about the cost?
7. Does the material support the key judgments?
8. Did the writing scatter the main thread for the sake of coverage?

Finally ask yourself: can the reader absorb one thing after finishing?
</content>
