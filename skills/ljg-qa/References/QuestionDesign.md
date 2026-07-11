# Question Design

How to make Q cut to the crux, and A not fall apart.

## The Four Types of Q (action-contrast-cause-boundary)

Each type corresponds to a pivot point in the author's argument. A good Q chain mixes at least three types.

| Type | Pattern | Example |
|------|------|---|
| *Action* | "How did they pull this off?" | "How does it turn X into Y?" |
| *Contrast* | "Why A and not B?" | "Why iterate instead of running in parallel?" |
| *Causation* | "Why does this solution hold?" | "Why does chain-of-thought reasoning emerge?" |
| *Boundary* | "When does it fail?" | "Does this approach still hold when data is sparse?" |

Why mixing matters:

- Pure action questions = a tutorial
- Pure contrast = a debate brief
- Pure causation = a paper
- Pure boundary = a reflection piece

Only a mix reproduces the real tension of "method + cost + limitation" that the original work has.

## Q Taboos

- ❌ "What is X?" — answered away with one definition, bears no weight
- ❌ "How many steps does X have?" — not really a question, it's asking for a table of contents
- ❌ "Is X important?" — the answer is presupposed, no tension
- ❌ "How should we think about X?" — academic tone, no concrete action
- ❌ "What are the pros and cons of X?" — business-school cliché
- ❌ "What does X mean for the future?" — can't be grounded

## Q's Tone

Don't dress it up. No "well then," no "let's go on to discuss." Cut straight in:

| Rewritten from | Rewritten to |
|--------|--------|
| How should we think about the issue of token economics? | Which step is the money actually worth spending on? |
| In AI engineering practice, is parallel processing appropriate? | Where does parallel mass-attempting go wrong? |
| What is the core mechanism of this method? | Why does it even work? |
| What are the limits of this method? | Where does it fall over? |

Colloquial beats academic. Under 20 words beats a long compound sentence.

## A's Four Parts

```
*Conclusion*: (one sentence — able to be quoted out of context)
*Formalization*: (one visualizable line of the relationship, in words)
*How you got there*: (2-4 short-sentence reasoning steps)
*Boundary*: (when it doesn't hold / what it doesn't cover)
```

### The Conclusion Sentence's Hard Requirement

One sentence, quotable out of context. If the reader forwards this sentence to a friend, the friend gets what you mean.

- ✓ "Depth costs more than breadth, but only depth buys you insight."
- ✓ "The reward signal locks the model onto trajectories it already knows."
- ✗ "Overall, tokens should be spent with some care."
- ✗ "This method shows certain advantages across multiple dimensions."

### The Formalization's Hard Requirement

Compress the idea into one visualizable line using words + simple symbols. It's "the geometry of the idea," not "the formalism of mathematics."

Allowed symbols: `= ≠ → ← + - × ÷ < > ⊃ ⊂ ⊥ ∧ ∨` + natural language + ASCII. No LaTeX, no complex expressions.

Four common patterns:

| Pattern | Example | Fits |
|------|-----|------|
| *Equation* | `generalist = coordinator; specialist = the one doing the work` | Assigning roles to concepts |
| *Contrast* | `old: big model = does everything; new: big model = coordinator` | Flipping the default frame |
| *Flow* | `data → tokens → answer = loss + waste` | Exposing pipeline loss |
| *Escalation* | `call → interface → bilingual hotline` | Tracing to the root cause |

Can also be a short formula or a small ASCII diagram (basic characters only, no Unicode drawing symbols):

```
depth = 1 agent × 100 steps > 100 agents × 1 step
```

Or:

```
breadth: ── ── ── ── (shallow taste)
depth:   |
         v (drill through)
```

Test for the formalization: pull it out on its own and show it to someone who hasn't read the A — can they get the gist? Yes → it passes. Needs explanation → rewrite it.

### The Reasoning Steps' Hard Requirement

Each short sentence takes exactly one step of reasoning. The previous step must open the door to the next.

- ✓
  - 100 agents each think for 1 step; expected hit rate ≈ 1/50
  - 1 agent thinks for 100 steps, each step's output feeding the next
  - Same total token budget, different hit rate

- ✗ "In multi-agent collaboration scenarios, an increase in concurrency doesn't necessarily bring an improvement in quality, because quality is closely tied to depth of thought, and depth of thought requires iteration over a time sequence..."

  (a whole paragraph lumped together = not broken apart = untraceable)

### The Boundary's Hard Requirement

A test of honesty. Every conclusion has somewhere it doesn't hold. Say it.

- ✓ "This assumes the task goal is stable and repeatedly verifiable. Open-ended exploratory tasks (like 'write a poem') don't necessarily need the depth route."
- ✓ "Doesn't hold for small models — small models hit diminishing returns from iteration very quickly."
- ✗ "(not written)" — makes the A look like an absolute truth when it's actually fragile
- ✗ "Future work could explore more dimensions." — PR-speak, not a boundary

Boundary ≠ shortcoming. Boundary is a "condition," a shortcoming is a "judgment." We write boundaries.

## The Topology of the Q-A Chain

Not a list — a path. Sketch it out first when designing:

```
Q1 ─┬─→ Q2 (goes deeper on Q1's answer)
    └─→ Q3 (the contrast Q1 raises)
Q2 ──→ Q4 (what Q2's boundary hints at)
Q4 ──→ Q5 (closing counter-question)
```

The reader walking from Q1 to Q5 is, in effect, reproducing the author's reasoning path.

*Random order = FAQ. Dependency = reasoning path.*

## Restraint on Quantity

5-10 Q's is the sweet spot.

- < 5: incomplete coverage, the reader feels undersatisfied
- > 10: reader fatigue, the tension dissipates
- 7 ± 2 is most comfortable — enough to hold the author's core argument without dragging on

If the source material is dense with ideas (a book, a dense paper), don't force it into one set — split by theme into multiple Q-A sets, each with 5-10 Q's.

## Self-Check List

After finishing the whole piece, go through this one by one:

- [ ] Every Q can withstand "could this be waved away with one definition"
- [ ] Every A is strictly four parts (conclusion / formalization / steps / boundary)
- [ ] The conclusion sentence is still quotable out of context
- [ ] The formalization is visualizable in one line — someone who hasn't read the A can still get it
- [ ] Every reasoning step takes exactly one step
- [ ] The boundary states "conditions under which it doesn't hold," not "future work"
- [ ] The Q chain has a sense of direction (removing one Q would collapse what follows)
- [ ] The Q types mix at least three kinds
- [ ] No "what is X"-type Q's
- [ ] Every Q sentence ≤ 20 words
- [ ] Total of 5-10 Q's
- [ ] Written in the reader's native language, no academic tone

Fail any of these → go back and fix it.
</content>
