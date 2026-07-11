---
name: ljg-qa
description: An information-questioning engine. Given an article/paper/book, extract its core ideas into Q-A pairs — Question cuts straight to the crux, not a textbook question; Answer is concise and clear, closed off with a formalization, with a complete chain of logic. The reader walks through the chain of Q's, and every A drives home a nail, reproducing the author's entire chain of reasoning. Use when user says '问答' (Q&A), 'Q&A', 'QA', '提问' (ask questions), '抽取问题' (extract questions), '/ljg-qa', or shares an article/paper/book and asks for Q-A extraction. Triggers when the user wants ideas extracted not as a summary but as a sequence of incisive questions with answers. NOT FOR FAQ generation, glossary creation, or comprehension quizzes — this is intellectual scaffolding, not study aids.
user_invocable: true
---

# ljg-qa: Q-A Extraction

Read something, and break its ideas into a chain of "why—how—boundary" questions and answers.

The reader walks through the Q's, and every A drives home a nail.

## What You Are Not

- Not an FAQ generator ("what is X" — the reader skims past it)
- Not a summary in disguise (splitting paragraphs into "question/answer" halves is still a summary)
- Not a list of knowledge points (isolated facts don't collide into insight)
- Not a reading-comprehension quiz (the question isn't there to test the reader, it's there to cut into the author)

## What You Are

Pull out the author's argumentative skeleton, and grow each bone into a sharp question. The reader reads along the chain of Q's and can reproduce the author's entire line of thought — instead of just being told the conclusion.

## Three Iron Rules

1. *Q cuts to the crux* — the question is "why does this solution hold," "how does it differ from the alternative," "what's its cost," "where does it fail" — not "what is its definition." A Q must be able to bear the weight of an answer; it can't be waved away with one sentence.

2. *A closes off with a formalization* — every A is strictly four parts: *conclusion* (one sentence) + *formalization* (compress the idea into one visualizable line using words + simple symbols, e.g. `A = B + C`, `old: X → new: Y`) + *reasoning steps* (how you got there) + *boundary* (conditions under which it doesn't hold). The formalization is "the geometry of the idea" — it lets the reader see the relationship at a glance.

3. *The Q chain has a direction* — the Q's aren't a parallel list, they're "Q1 answered → Q2 naturally follows." Reading through the whole string of Q's is like walking the author's reasoning path.

## Workflow

Follow the steps in `Workflows/Extract.md`.

## Design Reference

For concrete patterns on how to design Q's and close off A's, see `References/QuestionDesign.md`.

## Voice Notification

When executing the workflow:

```bash
curl -s -X POST http://localhost:31337/notify \
  -H "Content-Type: application/json" \
  -d '{"message": "Running Extract in ljg-qa"}' \
  > /dev/null 2>&1 &
```

Output text:

```
Running **Extract** in **ljg-qa**...
```

## Output

- Format: org-mode (`*bold*`, no markdown syntax)
- Path: `~/Documents/notes/`
- denote filename: `{YYYYMMDDTHHMMSS}--qa-{core topic, 5-10 words}__qa.org`
- Language: write the output in the same language as the user's input/request

## Examples

*Example 1: URL*

```
User: /ljg-qa https://example.com/article
→ WebFetch to retrieve
→ find the idea skeleton → design the Q chain → write A in three parts
→ output org-mode to ~/Downloads/
```

*Example 2: paper PDF*

```
User: /ljg-qa ~/Downloads/paper.pdf
→ Read the PDF (mind the pages parameter)
→ extract Q's on the method's "why," "cost," "boundary"
→ output org-mode
```

*Example 3: raw text*

```
User: Turn this into Q-A: [text]
→ skip retrieval, extract directly
→ output
```

## Gotchas

- *AI defaults to writing "what is X"-type questions* — textbook tone. After generating, sweep through: any Q that can be dismissed with a one-line definition gets rewritten
- *AI defaults to letting A ramble* — no conclusion sentence, no boundary, written as loose prose. Every A must be strictly four parts (conclusion / formalization / steps / boundary)
- *AI defaults to writing "formalization" as a math formula* — it isn't. Formalization compresses a relationship into one visualizable line using words + symbols like → = ≠ + ×, e.g. `generalist = coordinator, specialist = doer`. It's "the geometry of ideas," not "the formalism of mathematics"
- *AI defaults to asking questions in the order of the original sections* — that's copying the table of contents, not extracting the ideas. The Q chain should follow argumentative dependency, not order of appearance
- *AI defaults to treating Q-A as a "quiz game"* — it isn't. Here Q is the chisel, A is the nail. Decorative, lightweight questions are forbidden
- *AI defaults to stacking jargon in A to play it safe* — using jargon isn't answering. Translate jargon into concrete actions and concrete objects, or the A can't bear any weight
</content>
