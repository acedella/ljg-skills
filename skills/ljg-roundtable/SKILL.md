---
name: ljg-roundtable
description: >-
  One topic, one roundtable (圆桌讨论): the moderator invites 3-5 real
  historical/contemporary figures, opens with a definition, runs
  round after round of exchange, and closes each round with an ASCII
  structure diagram. The user controls the pace with commands
  (continue/stop/go deeper here/introduce new figure), and the full
  transcript is saved as a Markdown note in the Obsidian vault when it wraps up. Use when user
  says "圆桌讨论" (roundtable discussion), "圆桌" (roundtable),
  "roundtable", "辩论" (debate), or wants to explore a topic through
  multi-perspective structured debate.
---

## Usage

<example>
User: Roundtable discussion — does artificial intelligence have real creativity?
Assistant: [Moderator opens, invites the figures, starts from the definition]
</example>

<example>
User: Roundtable — does free will exist?
Assistant: [Runs a roundtable on free will]
</example>

## Instructions

First read `references/original-prompt.org` — this framework's original design is in there; the moderator's personality and the whole flow come from it.

### 1. Set the topic

If the user gave a topic, use it directly. If they only said "roundtable discussion" with no topic, ask first.

### 2. Invite the figures

Pick 3-5 real figures suited to the topic — historical or contemporary, either is fine, don't invent anyone. Give four things for each:

- Name
- MBTI
- Stance, in one sentence
- Why invite them: the angle they bring on this topic that nobody else can

Three criteria for picking figures:

- Their stances need to genuinely clash with each other — just splitting into a pro side and a con side isn't enough
- Prioritize people who've written something canonical or said something famous on this exact topic
- At least one outsider to the field

### 3. Opening

The moderator opens, reveals the lineup and the reasons for inviting each person, then throws out the defining question:

> "Before we start, let's nail down the core concept of the topic: what does it refer to? What elements can't be left out?"

Each person speaks in turn on the definition. The speech format is uniform across the whole session:

```
[Name] [Action tag]: speech content

**In short**: one-sentence summary
```

Six action tags: `Statement (陈述)`, `Question (质疑)`, `Addition (补充)`, `Rebuttal (反驳)`, `Revision (修正)`, `Synthesis (综合)`.

### 4. Discussion loop

Three steps each round.

**Speaking.** Who speaks is decided by the flow of the discussion, no fixed rotation. Every statement must follow on from what came before — questioning, adding to, or rebutting it — no talking past each other. Each statement ends with the usual **In short** line.

**Summary.** Once a round is done, the moderator does three things:

- Names the round's core point of contention. Pick the deepest one, don't try to cover everything.
- Draws an ASCII diagram: a 2x2 matrix, a spectrum axis, a causal loop, a hierarchy tree — whichever fits this round's structure. The diagram maps the structure of the discussion: how stances are distributed, how the causality loops around, where the feedback loops are. Don't use the diagram to just restate content.
- Draws out the next-level question from the point of contention.

**Waiting for a command.** After the summary, show the menu:

```
[Moderator]: (Command: Continue (可) / Stop (止) / Go deeper here (深入此节) / Introduce new figure (引入新人物))
```

- `Continue (可)`: accept the next-level question, move on
- `Stop (止)`: wrap up, move to the closing summary
- `Go deeper here (深入此节)`: don't advance, keep digging around the current point of contention
- `Introduce new figure (引入新人物)`: the user names a new figure, the moderator introduces them and asks them to weigh in on the current topic first

### 5. Closing

Once the user issues `Stop (止)`:

- The moderator gives an overall summary
- Draws one complete knowledge-network ASCII diagram: the key concepts of the discussion, everyone's stances, the points of contention, and how they connect
- Lists the open questions: directions that came up in the discussion but weren't fully explored

### 6. Archiving

The full discussion transcript goes into a Markdown file, word for word. Speeches, ASCII diagrams, summaries — all recorded verbatim, not summarized, not compressed, not rewritten.

1. Get a timestamp with `date +%Y%m%dT%H%M%S`
2. Write to `"/Users/jjin/Documents/Obsidian Vault/Roundtable Notes/{timestamp}--roundtable-{topic keywords}__roundtable.md"` (quote the path — it has spaces; create the folder if it doesn't exist)
3. File structure:

   ```markdown
   ---
   title: "Roundtable: {topic}"
   date: {date}
   tags: [roundtable]
   ---
   # Topic and participants
   [Lineup: name, MBTI, stance, reason for inclusion]
   # Opening: definition
   [Moderator's opening remarks + each person's opening definition]
   # Round-by-round record
   ## Round N: {guiding question}
   ### Speech record
   [All speeches from this round, verbatim, with action tags and In-short lines]
   ### Moderator's summary
   [Point of contention + ASCII diagram + next-level question]
   # Knowledge network (overall)
   [Overall summary + knowledge-network diagram]
   # Open questions
   [Directions left unresolved]
   ```

   ASCII diagrams must be wrapped in fenced code blocks so Obsidian renders them intact.

4. Once done, report the file path to the user.

### How the moderator should act

- Don't take sides.
- Chase only one point of contention per round, chase it to the bottom, don't spread thin.
- Push for real clashes. If participants are being polite and dodging disagreement, the moderator should call it out and put the disagreement back on the table. Surface agreement is the same as no discussion at all.
- When summarizing, lay each side's hidden cards on the table: what assumptions they're standing on, what their premises are, where the reasoning chains fork. Just restating who said what isn't enough.

### How participants should speak

- Stay faithful to the person's real system of thought — say what this person would actually say, cite books they've written, quotes they've actually said.
- No generic, safe-sounding truisms. If questioning, point out exactly which premise of the other side doesn't hold up; if adding on, push the argument one step further.
</content>
