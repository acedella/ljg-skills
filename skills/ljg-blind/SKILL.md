---
name: ljg-blind
description: "Blind spot scan (盲区扫描) — reads all of yesterday's conversations between you and AI, and illuminates a cognitive blind spot (not a knowledge gap, but a thinking habit that keeps some category of truth permanently invisible), then picks one precisely-matched chapter from WeRead (微信读书) to fill it, writing the whole thing up as a complete analysis note. Use when user says '扫盲区' (scan blind spot), '盲区' (blind spot), '照盲区' (illuminate blind spot), '看看我的思维盲区' (look at my cognitive blind spots), '我昨天想漏了什么' (what did I miss thinking yesterday), 'blind spot', 'ljg-blind', '/ljg-blind', or wants yesterday's AI conversations analyzed for cognitive blind spots plus a targeted WeRead chapter to fill them. Optional parameter: pass a date (YYYY-MM-DD) to scan that day instead of yesterday. NOT FOR drilling vertically into a single viewpoint (use ljg-think), reducing the order of a domain (use ljg-rank), reading alongside a text (use ljg-read), or finding the constraints of a domain (use ljg-constraint)."
user_invocable: true
---

# Blind Spot Scan

Read all of yesterday's conversations with AI, illuminate the thinking blind spot you can't see yourself, and light up a chapter from WeRead (微信读书) that fills it.

## What a blind spot is

A blind spot isn't a knowledge gap. It's not "haven't read some book" or "don't know some fact" — that kind of gap gets closed the moment you look it up.

A blind spot is a structural thinking habit that makes some category of truth **systematically invisible** to you. It's not that you couldn't think of it — this habit means you never even looked in that direction. It hides in how you think about things, not in what you think about.

So the evidence isn't in "what he got wrong." It's in — where he never looked, where he circled without landing, what he defaulted to but never checked.

## Five blind-spot signals

Look for these five in yesterday's conversation. Each one must land on a specific thing said — not a gut feeling.

1. **Avoidance point** — a hard problem he opened, then quickly shut. Evidence: a question gets raised, then the topic changes, or a "let's not worry about that for now" slides right past it.
2. **Idling framework** — switching perspectives on the same problem again and again, none of them landing. Evidence: one problem gets three or four frameworks slapped onto it, each explored only shallowly, ending without a conclusion.
3. **Single viewfinder** — a whole day spent calling on only one or two f's (defaulting to "constraint," "evolution," "game theory" for everything), never trying a different lens. This is his most hidden blind spot: the more comfortable an f becomes, the more it blocks what other f's could reveal. (Jigang sees the world as world = f(x); the four axes are his default four f's — and precisely because they're the default, it's most worth checking whether today he again only used one of them.)
4. **Unchecked premise** — a default assumption running through the whole conversation, being used as a hard constraint, never once questioned as to whether it's really a fake wall. Evidence: some "this is just how it should be" statement, repeated but never once questioned by himself.
5. **Adjacent gap** — working backward from what he did ask, the corner that should have been asked about but wasn't. Not a guess out of nowhere — his own clues point there.

## Which one to pick

Pick **1** of the signals as today's blind spot. Better to say one thing thoroughly than to touch all five — blind-spot density matters less than precision. Three criteria:

- **Leverage** — filling it opens the most possibilities for the next step.
- **Genuinely blind** — something he truly can't see, not something he already knows and is procrastinating on.
- **Aligned with the mission** — matches M0 (finding a new viewfinder) / M1 (seeking the essence). If the blind spot happens to be blocking his main line, prioritize it. Read `~/.claude/PAI/USER/TELOS/MISSION.md` to confirm the main line.

---

## Operating steps

### Step 1 · Set the date

```bash
# Defaults to yesterday; if the user passed YYYY-MM-DD, use that day (macOS BSD date)
target=${1:-$(date -v-1d +%Y-%m-%d)}
```

### Step 2 · Pull that day's conversations

Session files live at `~/.claude/projects/-Users-lijigang*/*.jsonl`, one message per line (type=user / assistant / system). Keep only Jigang's actual human utterances, filtering out tool echoes and subagent noise:

```bash
target=<date from previous step>
out=/tmp/ljg-blind-${target}.txt
: > "$out"
# Only take top-level session files (excluding subagents/workflows), keep only plain human text
for f in $(rg -l "\"timestamp\":\"${target}" ~/.claude/projects/-Users-lijigang*/ 2>/dev/null | grep -vE 'subagents/|workflows/'); do
  jq -r 'select(.type=="user" and ((.timestamp // "") | startswith("'"${target}"'"))) |
    (.message.content) as $c |
    (if ($c|type)=="string" then $c
     elif ($c|type)=="array" then ([$c[] | select(.type=="text") | .text] | join(" "))
     else "" end) as $t |
    select($t|length>0) |
    "[" + (.timestamp|.[11:16]) + "] " + $t' "$f" 2>/dev/null >> "$out"
done
# Filter out tool echoes / system noise / this very invocation's own request
grep -vE 'tool_use_id|system-reminder|caveat|Caveat|local-command|command-name|command-message|<task-notification>|ljg-blind|扫盲区' "$out" > "${out}.f" && mv "${out}.f" "$out"
wc -c "$out"
```

If the material exceeds 50KB, read it in segments by conversation, first identifying what topic each segment discusses before going further. You can also glance at a few adjacent assistant replies to establish context.

**Thin-data fallback**: if that day's human input is under 200 characters or there's no conversation at all — say so directly in the output, "conversation that day was thin / absent," **don't force a blind spot**. Better to turn in a blank page than fabricate one.

### Step 3 · Read out the blind spot

Go through the "Five blind-spot signals" above, and for every hit, note the evidence (which specific lines, what turn made you see it). Then use the three criteria in "Which one to pick" to select the **1** most important blind spot.

### Step 4 · Pick a WeRead chapter

Call the weread skill (`WEREAD_API_KEY` should be in the environment, format `wrk-xxxx`; if missing, prompt `export WEREAD_API_KEY=<key>`). Three steps:

1. **Search for a book** — pull 1-2 core terms from the blind spot as the keyword:

```bash
curl -s -X POST https://i.weread.qq.com/api/agent/gateway \
  -H "Authorization: Bearer $WEREAD_API_KEY" -H "Content-Type: application/json" \
  -d '{"api_name":"/store/search","keyword":"<core term>","count":8,"skill_version":"1.0.3"}' \
  | jq -r '.results[].books[]?.bookInfo | "\(.bookId)\t\(.title)\t\(.author)\trating \(.newRating)"'
```

From the results, pick the 1 book that scores as close to ≥750 as possible (the new rating scale is ×100, i.e. 7.5) and matches the blind spot best, and note the bookId. Rating isn't the only criterion — a match beats a high score: a 7.4 book that hits the blind spot dead on beats an 8.5 book that only grazes it.

2. **Check the table of contents** — pick the single **1 chapter** that fits best, not the whole book, not a vague preface:

```bash
curl -s -X POST https://i.weread.qq.com/api/agent/gateway \
  -H "Authorization: Bearer $WEREAD_API_KEY" -H "Content-Type: application/json" \
  -d '{"api_name":"/book/chapterinfo","bookId":"<bookId>","skill_version":"1.0.3"}' \
  | jq -r '.chapters[] | select(.level<=2) | "\(.chapterUid)\t[\(.level)] \(.title)\t\(.wordCount) words"'
```

Note the chapterUid, chapter title, and wordCount.

3. **Estimate the time** — `wordCount / 280` (Chinese reading speed), rounded up to the nearest multiple of 5. Without a wordCount, estimate 15-45 minutes by feel from the table of contents; if the title has macro-scale words like "Part 1/Part 2" or "Volume One," multiply the estimate by 1.5.

4. **Build the link**: `weread://reading?bId={bookId}&chapterUid={chapterUid}`

**Fallback**: if the search comes up completely empty / there's no matching chapter → pick the whole related book instead, fall the link back to `weread://reading?bId={bookId}`, and explain in the text why it couldn't be narrowed down to a specific chapter.

### Step 5 · Write the note

Get timestamps: `date +%Y%m%dT%H%M%S` and `date "+%Y-%m-%d %a %H:%M"` (use the current time, not the target date).

Write to `~/Documents/notes/{timestamp}--blind-{topic}__blind.org`. Org-mode format, no markdown syntax allowed.

Body template:

```org
#+title: Blind Spot Scan · {one sentence naming this blind spot}
#+date: [YYYY-MM-DD Weekday HH:MM]
#+filetags: :blind:weread:topology:

* What you were thinking about yesterday
<1-2 paragraphs. The terrain of yesterday's conversation — which few things, circling which core. Give evidence: which specific lines show this. Not a play-by-play — grab the main thread.>

* The blind spot revealed
<The 1 blind spot picked out. First, one sentence naming which type it is (avoidance point / idling framework / single viewfinder / unchecked premise / adjacent gap). Then 2-3 paragraphs spelling out: what it specifically looks like, which moments yesterday exposed it, why he can't see it himself. Must name the leverage — what opens up once it's filled.>

* This chapter is for you
- Book: "Title" — Author
- Chapter: <chapter title>
- Time: about N minutes
- Link: [[weread://reading?bId=XXX&chapterUid=YYY][Open in WeRead]]
- Why this one: <3-4 sentences. Match the blind spot to this chapter — what this chapter specifically covers, how it fills this blind spot. Don't say vague things like "broadens perspective" — spell out exactly which part of this chapter addresses exactly which gap in the blind spot.>
```

Report the note's file path to the user.

---

## Tone rules (hold to these when writing the note)

This note is written for someone who has been thinking alongside you for a long time — it's not a report, not for a stranger reader.

- **Write in the same language as the input/request; no translation-ese.** If writing in Chinese: avoid stiff connectives like 然而/此外/值得注意的是 (however/moreover/it's worth noting); avoid over-nominalization (对…的理解 → 懂… "understanding of X" → "get X", 做出…的决定 → 决定 "make a decision to..." → "decide"); drop the subject where possible; prefer short sentences. Read it back silently and ask: would someone who's never read a translated novel talk like this? Would Wang Zengqi (汪曾祺, a Chinese writer known for plain, natural prose) write it this way? If writing in English, apply the equivalent test: would a sharp, plainspoken writer actually say it like this out loud? Rewrite the whole passage where it snags — don't just swap a word.
- **No "sharpened blade" metal metaphors.** Don't write things like "this cut," "one level fiercer," "sharp words," "nail it down," "lock it in" — this whole self-congratulatory rhetorical toolkit. Whether a blind spot is named accurately shows in the reading; it doesn't need the author shouting about how hard he swung.
- **Have warmth, have rough edges, have judgment.** It's fine to take a stance — don't fake neutrality to the point of dishonesty; but label clearly when something is judgment, not objective fact.

## Strict org syntax (don't mix in markdown)

- Headings use `*` / `**` / `***`, not `#`
- Bold `*text*`, italic `/text/`, monospace `~code~`
- Lists use `-`, not `*` (`*` is a heading in org)
- Links `[[url][text]]`, not `[text](url)`
- Dividers `-----`, not `---`; no markdown `>` blockquotes

## Self-check (run through before finishing)

- Does the blind spot have concrete evidence? Can every point be traced back to specific lines from yesterday? If not, go back to Step 3.
- Did you pick only 1 blind spot and say it thoroughly? Or greedily touch three or four?
- Does the chapter link have both bookId and chapterUid non-empty, correctly formatted?
- Checked for translation-ese? Scan for over-nominalization, stilted constructions, droppable connectives.
- Checked for sharpened-blade metal metaphors? Not one "this cut," "nail it down," "lock it in" left.
- Do all three sections have content, no empty sections?

## Division of labor with the "morning letter"

Pulse sends an automatic morning letter every day (`陈平安思维拓扑.org`, "Chen Ping'an's Thinking Topology"), written in Chen Ping'an's voice, one per day, picking one hole to point at. ljg-blind is the **on-demand** analysis version — same underlying process of "read yesterday → see the blind spot → pick a WeRead chapter," but more systematic, focused purely on blind spots, written with fuller analysis, and able to scan any given day (pass a date parameter). Want a letter — wait for the morning letter; want to proactively check a given day's blind spot, want an analysis piece you can archive — use ljg-blind.
</content>
