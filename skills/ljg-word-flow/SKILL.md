---
name: ljg-word-flow
description: "Word flow: deep-dive word analysis + infograph card in one go. Takes one or more English words, runs ljg-word (generates deep semantics analysis) then ljg-card -i (generates infograph PNG). Use when user says '词卡', 'word card', 'word flow', or provides English words wanting both analysis and visual card."
user_invocable: true
version: "1.0.1"
---

# ljg-word-flow: Word Card (词卡)

One command does it all: word deconstruction → cast infograph. Supports parallel processing of multiple words.

## Mode

**NATIVE mode is enforced.** This workflow is a pure skill pipeline (ljg-word → ljg-card -i) and does not need the Algorithm's seven-step process. Call the skills directly according to the execution steps below, without going through OBSERVE/THINK/PLAN/BUILD/EXECUTE/VERIFY/LEARN.

## Parameters

Pass in one or more English words directly, separated by spaces.

```
/ljg-word-flow Obstacle
/ljg-word-flow Serendipity Resilience Entropy
```

## Execution

### 1. Collect the Word List

Extract all English words from the user's message.

### 2. Process Each Word

For each word, execute two steps in sequence:

**Step A — Word Deconstruction (ljg-word):**

Invoke the Skill tool to run `ljg-word`, passing in the word. Output the Markdown analysis result in the conversation.

**Step B — Cast Infograph (ljg-card -i):**

Using the analysis content from Step A as input, invoke the Skill tool to run `ljg-card -i`. Generate a PNG file to `~/Downloads/`.

### 3. Parallel Processing for Multiple Words

When there are multiple words, launch one Agent subagent per word to process in parallel (within each subagent, A→B runs sequentially).

### 4. Summary Report

```
════ Word Cards Complete ═══════════════════════
📖 {Word1}
   🖼️ ~/Downloads/{Word1}.png

📖 {Word2}
   🖼️ ~/Downloads/{Word2}.png
...
```

## Key Constraints

- Deconstruct the word before casting the card; the order cannot be reversed
- The quality standards for ljg-word and ljg-card -i each remain unchanged
- The infograph content comes from the word deconstruction result, not a dictionary definition
