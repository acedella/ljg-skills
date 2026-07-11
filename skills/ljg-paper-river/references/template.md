---
title:      paper-river-{short title}
date:       [{YYYY-MM-DD Day HH:MM}]
tags:       [paper, river]
identifier: {YYYYMMDDTHHMMSS}
source:     {paper URL or source provided by the user}
authors:    {target paper's authors}
venue:      {publication venue/year}
---

# The river of the problem

{One sentence sketching this research line's core problem — the kind that makes even an outsider want to know the answer.}

{Why is this problem hard? Where does it get stuck? Why have generations of researchers kept charging at it?}

# Tracing map

{ASCII diagram: draw the evolution chain between the papers.}

{Format example:}

```
[2017] Vaswani et al. - Attention Is All You Need
  |
  | Problem: RNNs are too slow; sequences must be computed one step at a time
  | Solution: drop the RNN, pure attention
  v
[2018] Devlin et al. - BERT
  |
  | Problem: the Transformer only looks in one direction
  | Solution: let it look both left and right (bidirectional)
  v
[2020] Brown et al. - GPT-3
  ...
```

# Evolution narrative

{Start from the oldest paper and, with the problem as the through-line, narrate all the way to the target paper.}

{One subsection per paper, structured:}

## {year} | {paper short name}: {one sentence on what it did}

### What problem it saw

{Where did the predecessors' solution fall short? Be specific about which scenario breaks in what way.}

### Its solution

{What's the core idea? Explain it with an analogy or example.}

### Why the move works (and where it buries a new pitfall)

{The intuition behind the solution. Also point out the limitation it leaves behind — that limitation is the next paper's starting point.}

# Frontier extensions

{After the target paper, who has pushed this problem further?}

{If follow-up papers are found, use the same structure. If none, say "as of now, this is the latest progress on this line."}

# One diagram to see it all

{Draw an ASCII "problem-solution" evolution diagram compressing the whole line into one screen.}

{Horizontal axis is time; vertical axis is the different solution dimensions. One glance should show how this field grew.}

# Insight

{Having read the whole evolution line, what do you see?}

{Not a restatement of each paper's conclusions — the pattern you yourself see: what is really changing beneath this line? Where is it most likely to go next?}

# Implications

{What does this evolution line suggest about "how to do research" and "how to find problems"?}

{Land on "usable": this means you can ___.}
