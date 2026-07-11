---
name: ljg-word
description: Deep-dive English word mastery tool. Deconstructs a single English word into core semantics and epiphany. Use when user asks to explain/master a specific English word.
version: "1.0.1"
user_invocable: true
---

## Usage

<example>
User: Deeply explain the word "Serendipity".
Assistant: [Calls ljg-explain-words with "Serendipity"]
</example>

## Instructions

The goal is not translation, but to help the user master the deep meaning and usage of this word.

For the input `word` (convert to lowercase, then capitalize the first letter), perform the following analysis and output it directly in the conversation using Markdown:

### Output Structure

#### 1. Title Line

```
## {Word}  /{IPA}/  {Chinese translation}
```

#### 2. Core Semantics

- **Original Image**: Describe in one sentence the most physical image at the root of this word (e.g., Incubate: a hen sitting on eggs).
- **Core Image**: Distill it into a formula (e.g., Warmth + Time + Protection = Incubation).
- **Explanation**: Use insightful language to elaborate on its deep meaning and modern usage. Clear paragraphing, **bold** key words. It should have penetrating depth, revealing the inner connections between etymology and meanings across multiple domains.

#### 3. One-Line Epiphany (一语道破)

A bilingual (English/Chinese) golden sentence that must have philosophical depth, summarizing the soul of the word. Use blockquote format:

```
> "English sentence. Chinese golden sentence."
```
