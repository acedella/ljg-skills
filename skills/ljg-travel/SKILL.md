---
name: ljg-travel
description: "Deep travel research workflow for museums and ancient architecture. Input a city name, auto-generates structured knowledge document (org-mode) + portable reference cards (PNG). Covers historical background, museum highlights, archaeological significance, and architectural heritage. Use when user says '旅行研究' (travel research), '博物馆功课' (museum homework/prep), '古建功课' (ancient-architecture homework/prep), 'travel research', '出发前功课' (pre-departure homework/prep), or provides a city name with intent to do deep cultural travel preparation."
user_invocable: true
version: "1.0.0"
---

# ljg-travel-flow: Travel Research (旅行研究)

One command completes: full-dimension cultural research -> content distillation -> org document + portable cards.

Methodology borrowed from archaeology's Desk-Based Assessment (DBA): exhaust all documentary evidence before arriving.

## Mode

**Forced NATIVE mode.** This workflow is a multi-skill pipeline (Research -> ContentAnalysis -> ljg-card); it does not go through the Algorithm's seven-step process.

## Parameters

| Parameter | Description | Example |
|------|------|------|
| City name | Required, target city | Xi'an, Luoyang, Datong |
| `-f` | Focus theme (optional) | `-f Tang dynasty` `-f cave temples` `-f bronzeware` |
| `-q` | Quick mode, skip content distillation, only do research + document | |

## Execution

### 1. Parse parameters

Extract the city name and optional parameters from the user's message. If there's a focus theme, all subsequent searches revolve around it.

### 2. Full-dimension research (Research extensive — single call, 12 agents in parallel)

Call the Skill tool to run `Research`, using extensive mode.

**Core design: don't split into "knowledge baseline" and "platform discovery" as two steps — they're different search angles of the same research operation.** 12 agents launch simultaneously, half doing academic/encyclopedic research, half doing platform content search.

**Research outline (passed as the prompt to Research):**

```
Do deep cultural travel research on "{city}". This isn't a travel guide — it's pre-departure, archaeology-style desk-based assessment (Desk-Based Assessment).

Research covers the following dimensions, each needing bilingual (native-language + English) search:

**Dimension A — Historical stratigraphy**
What major historical periods has this city gone through? What physical remains did each period leave behind in this city? How did dynastic change shape the city's layout?

**Dimension B — Museum highlights**
What major museums does this city have? What are each museum's signature treasures and core collections? Which exhibits carry major archaeological significance? Must give specific artifact names and gallery locations.

**Dimension C — Ancient architectural remains**
Which major ancient buildings and sites still exist? Construction date, architectural form, structural features. Which are national key cultural heritage protection sites? What details should you pay attention to when viewing the building (bracket sets/dougong, painted decoration, inscribed steles, etc.)?

**Dimension D — Archaeological discoveries**
What major archaeological discoveries have been made in and around this city? Which museums now hold the unearthed artifacts? Are there interesting stories from the excavation process?

**Dimension E — Cultural context**
Important historical figures, literary works, and cultural traditions associated with this city. Helps understand the city's cultural character.

**Dimension F — Deep-content discovery**
Search Bilibili (bilibili.com), Zhihu (zhihu.com), WeChat official accounts (mp.weixin.qq.com), Douyin (douyin.com), and Xiaohongshu (xiaohongshu.com) for in-depth explainer content about this city's museums and ancient architecture.
Filtering criteria:
- Want: content with real knowledge increment (explains background, craftsmanship, the excavation process, architectural detail)
- Don't want: pure check-in photos, pure recommendations with no analysis, ad copy
- For Bilibili videos, prefer explainer-style videos over 10 minutes
- For official-account articles, prefer ones with references or a clearly identified author
Return the content title, URL, and a one-sentence summary.

{If there's a focus theme: pay special attention to content related to "{focus theme}", with the other dimensions as background support.}
```

Wait for Research to finish, and get the full-dimension research results.

### 3. Content distillation (ContentAnalysis — optional)

Skip this step if the user specified `-q` quick mode.

From the results returned in step 2, extract all valid URLs (article links, video links).

**Launch a parallel Agent subagent for each URL:**

Each subagent calls the Skill tool to run `ContentAnalysis`, passing in the URL, using the fast depth level, extracting core knowledge points.

**Degradation rules:**
- If ContentAnalysis fails on a URL (inaccessible, no subtitles, etc.), skip that URL, don't block
- If all URLs fail, the process doesn't stop — the research results from step 2 are already enough to generate the document
- ContentAnalysis is an enhancement layer, not a required layer

Collect all successfully distilled content summaries.

### 4. Synthesize the org-mode document

Synthesize the results of step 2 (research results) and step 3 (content distillation, if any) into one structured org-mode document.

**Document structure:**

```org
#+title: {City} Travel Research
#+date: {current date}
#+filetags: :travel:museum:architecture:

* City overview
  {City}'s civilizational coordinates — why it's worth visiting, what to see when you go. One paragraph outlining this city's place in the history of civilization.

* Historical stratigraphy
** {Period 1} ({era range})
   Core events, remaining traces, corresponding physical items you can see.
** {Period 2}
   ...

* Museum guide
** {Museum 1 name}
   Address, opening hours, reservation method (if needed).
*** Signature treasures
    - {artifact name}: {why it matters} | what detail to look at: {specific observation point}
*** Key galleries
    - {gallery name}: {core highlight}
*** Easy to miss
    - {overlooked but worthwhile content}
** {Museum 2 name}
   ...

* Ancient architectural remains
** {Ancient building 1 name} ({dynasty}, {protection level})
   Overview of its form.
*** What to look at
    - {specific observation point 1}: {why it's worth noting}
    - {specific observation point 2}
** {Ancient building 2 name}
   ...

* Archaeological discoveries
** {Site/discovery 1}
   How it was discovered, its significance, where the unearthed artifacts are now held. If there's an interesting excavation story, tell it.

* Visiting routes
** Route 1: {theme name} ({estimated time})
   Who it's for: {description}
   1. {location} -> focus on {what} ({suggested dwell time})
   2. ...
** Route 2: {theme name}
   ...

* Recommended deep content
  Content worth watching before departure, found across various platforms.
** Videos
   - [[{URL}][{title}]] — {one-sentence summary}
** Articles
   - [[{URL}][{title}]] — {one-sentence summary}
** Books (if recommended)
   - {book title} — {why it's worth reading}
```

**File naming**: use the denote naming schema, save to the `~/Documents/notes/` directory:
`{YYYYMMDDTHHMMSS}==z--{city} Travel Research.org`

**Writing requirements**:
- Every recommendation must have "why look at this" and "what detail to look at" — nothing vague allowed
- The tone is a note written for yourself, not a tour guide's spiel
- Write concrete facts when you have them, don't make things up when you don't

### 5. Forge portable cards (ljg-card)

Extract the core content from step 4's org document and forge two cards, **run in parallel**:

**Card A — City civilization overview (infographic):**

Call the Skill tool to run `ljg-card -i`, with input content: the city's historical stratigraphy + core museum list + must-see ancient architecture list, distilled to essentials. One image to understand this city's civilizational skeleton at a glance.

**Card B — Visiting route quick reference (long card):**

Call the Skill tool to run `ljg-card -l`, with input content: visiting route recommendations + the core highlights of each location. Checkable on your phone anytime.

### 6. Summary report

```
════ Travel Research Complete ═══════════════════════
🏛️ City: {city name}
📝 Knowledge document: {org file path}
🖼️ Civilization overview card: {PNG file path}
🖼️ Route quick-reference card: {PNG file path}
📊 Research coverage: {N} museums | {M} ancient buildings | {K} archaeological sites
📎 Deep content: {X} videos | {Y} articles
```

## Key constraints

- Step 2 is the core — 12 agents in parallel cover academic research and platform content in one pass
- Step 3 (content distillation) is an enhancement layer; failure doesn't block the process
- The two cards in step 5 run in parallel
- The org document is the primary output, the cards are derivative outputs — document quality comes first
- Don't produce a generic travel guide; every recommendation must have "why look at this" and "what detail to look at"
- Research searches use bilingual (native-language + English) keywords to widen coverage
- When there's no concrete information, leave it blank rather than making it up

## Known Pitfalls

(First created, no records yet. Accumulate through use.)
</content>
