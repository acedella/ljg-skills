---
name: ljg-paper-flow
description: "Paper workflow: read papers + cast viewfinder (取景框) library cards in one go. Takes one or more arxiv links, paper URLs, PDFs, or paper names. For each paper, runs ljg-paper (generates org analysis) then ljg-library (distills the paper's viewfinder (取景框) into a 2050 library card PNG). Use when user says '论文流' (paper flow), 'paper flow', '读论文并做卡片' (read papers and make cards), '论文卡片' (paper card), or provides multiple papers wanting both analysis and cards."
user_invocable: true
version: "1.1.0"
---

# ljg-paper-flow: Paper Flow (论文流)

One command does it all: read the paper → generate an analysis → cast it into a viewfinder library card. Supports parallel processing of multiple papers.

## Mode

**NATIVE mode is enforced.** This workflow is a pure skill pipeline (ljg-paper → ljg-library) and does not need the Algorithm's seven-step process. Call the skills directly according to the execution steps below, without going through OBSERVE/THINK/PLAN/BUILD/EXECUTE/VERIFY/LEARN.

## Parameters

No flags. Paper sources are taken from the conversation or command (arxiv URL, PDF path, paper name). Each paper goes through ljg-paper → ljg-library; the card type is fixed as a viewfinder library card — ljg-library only produces this one type of card, unlike ljg-card's set of `-l/-i/-c/-v` templates to choose from.

## Execution

### 1. Collect the Paper List

Extract all paper sources from the user's message (arxiv URL, PDF path, paper name, etc.).

### 2. Process Each Paper in Parallel

For each paper, launch one Agent subagent; each subagent executes two steps in order:

**Step A — Read the Paper (ljg-paper):**

Invoke the Skill tool to run `ljg-paper`, passing in the paper's source. Wait for completion to get the generated org file path.

**Step B — Cast the Viewfinder Card (ljg-library):**

Read the org file generated in Step A — it has already fully broken down this paper's proposition, its core, and the new way of seeing the world it brings, which is exactly the "fully-thought-through viewfinder idea" that ljg-library needs. Hand this idea, together with the paper's title / authors / arxiv info, to the Skill tool to run `ljg-library`: take its flexible-input path (when an idea is already given, it only validates + draws, skipping distillation from scratch), producing a viewfinder library card PNG. Wait for completion to get the PNG path.

### 3. Summary Report

Once all papers have been processed, output a summary:

```
════ Paper Flow Complete ═══════════════════════
📄 {Paper Title 1}
   📝 Analysis: {org file path}
   🃏 Viewfinder Card: {PNG file path}

📄 {Paper Title 2}
   📝 Analysis: {org file path}
   🃏 Viewfinder Card: {PNG file path}
...
```

## Key Constraints

- The two steps for each paper must run sequentially (paper first, then library), but multiple papers run in parallel with each other
- The quality standards, red lines, and taste guidelines for ljg-paper and ljg-library each remain unchanged
- The card's viewfinder comes from the generated org file (the fully broken-down proposition + core), not the original paper
- **Papers ≠ books, materials are downgraded**: ljg-library's cover (weread) / author avatar (Wikipedia) / publisher field are designed for books. Papers go through arxiv: weread usually can't find them → the cover falls back to a CSS placeholder; arxiv authors usually aren't on Wikipedia → the avatar is omitted; the bibliographic line uses arxiv metadata instead (authors · arXiv:ID · year). The card's essence lies in the viewfinder's imagery + Feynman-style explanation + hand-drawn illustration; papers fit this part perfectly, so the missing cover/avatar doesn't hurt the core.
- **Isolate /tmp contention during parallel runs**: by default, ljg-library hardcodes the cover / avatar to `/tmp/lib_cover.jpg` and `/tmp/lib_avatar.jpg`; running multiple papers in parallel will overwrite each other's images. Each subagent must use a unique path (e.g., `/tmp/lib_cover_{slug}.jpg`), and after rendering, visually verify the images to confirm nothing got mixed up.
