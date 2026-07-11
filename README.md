# ljg-skills

My custom skill set for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Installation

Install in one line using the [skills CLI](https://github.com/vercel-labs/skills) (based on `npx`):

```bash
# Install all skills (global, org-mode format)
npx skills add lijigang/ljg-skills -g --all

# Install all skills (Markdown format, for Obsidian / VSCode / Notion etc.)
npx skills add lijigang/ljg-skills#md -g --all

# Install a single skill
npx skills add lijigang/ljg-skills -g --skill ljg-card

# Install a single skill (Markdown format)
npx skills add lijigang/ljg-skills#md -g --skill ljg-card

# Install multiple specified skills
npx skills add lijigang/ljg-skills -g --skill ljg-card --skill ljg-learn

# List which skills are in the repo
npx skills add lijigang/ljg-skills -l
```

**Flag descriptions:**

| Flag | Effect |
|------|------|
| `-g` | Install globally to `~/.claude/skills/` (recommended). Without it, installs to the current project's `.claude/skills/` |
| `--skill <name>` | Install a specific skill; can be used repeatedly |
| `--all` | Install all skills in the repo |
| `#md` | Install the Markdown-format version from the `md` branch (org-mode is the default) |
| `-l` | Only list available skills, don't install |

### ljg-card Dependencies

`ljg-card` depends on Playwright for screenshots; after installing, run additionally:

```bash
cd ~/.claude/skills/ljg-card && npm install && npx playwright install chromium
```

### Alternative: git clone

```bash
# org-mode version
git clone https://github.com/lijigang/ljg-skills.git ~/.claude/plugins/ljg-skills

# Markdown version
git clone -b md https://github.com/lijigang/ljg-skills.git ~/.claude/plugins/ljg-skills
```

## Skills

| Skill | Description |
|------|------|
| **ljg-blind** | Blind spot scan — reads AI conversations from a specified date, identifies structural thinking blind spots, and fills them precisely using WeChat Reading (微信读书) chapters |
| **ljg-card** | Content-to-card casting — turns content into PNG visual cards (long image `-l`, infograph `-i`, multi-card `-m`, visual notes `-v`, comic `-c`, whiteboard `-w`, big text `-b`) |
| **ljg-learn** | Concept anatomy — cuts a concept open from eight directions (history, dialectics, phenomenology, linguistics, formalization, existentialism, aesthetics, meta-reflection), compressed into a single epiphany |
| **ljg-paper** | Paper reading — extracts a paper's core idea for non-academic readers, prioritizing understanding over critique |
| **ljg-paper-river** | Paper genealogy — reverse-reading method, recursively digs into prior papers (up to 5 levels) + latest developments, telling the evolution of the problem from its source |
| **ljg-book** | Book breakdown — lands on one equation f(x): what question is the author standing in front of (x) / what is this viewfinder (取景框) of his (f, the central question: a new frame conjured up, or a ready-made frame picked off the shelf) / the picture and conclusion illuminated through the frame (f(x)); ends with an ASCII reference-frame diagram pinning each f into the same picture, then pushes the frame two steps beyond the book to make falsifiable predictions |
| **ljg-library** | Viewfinder library card — one book → one "viewfinder" (取景框) image → one collectible card (PNG): real cover / author / bibliography + a Feynman-style explanation of the image; illustration is AI-generated, with Jigang (继刚) as the fixed protagonist (generated from an ink-portrait reference), in the picture-book style of Seiji Yoshida (吉田诚治) — an otherworldly-everyday picture-book feel, warm slanting light, healing yet refined |
| **ljg-map** | Ecosystem terrain map card — one industry → one bird's-eye ecosystem terrain (PNG, AI-generated, default `-a` Animal Crossing style / optional `-c` cyber): value flows like a river across the terrain, marking bottlenecks (narrowing passes/dams) and value-capture points (treasure piles where profit settles), with Jigang (继刚) as the surveyor looking down; paired with three key base-rate metrics + three big questions |
| **ljg-qa** | Information question machine — extracts the core arguments of an article/paper/book into a Q-A chain, with each Q cutting to the heart of the matter and each A in four parts (conclusion / formalization / steps / boundaries) |
| **ljg-plain** | Plain-language engine — rewrites any content so a smart twelve-year-old can understand it |
| **ljg-rank** | Rank-reduction engine — given a domain, finds the irreducible set of independent generators behind it |
| **ljg-constraint** | Constraint engine — given a domain/profession/role, finds the few constraints that box it in (three layers: hard/soft/self-imposed), calls out false walls mistaken for hard constraints, and points out which ones can be redefined |
| **ljg-think** | Root-tracing arrow (追本之箭) — given an opinion or phenomenon, drills vertically down to its irreducible essence |
| **ljg-word** | Word mastery — deeply deconstructs the core semantics and epiphany moment of an English word |
| **ljg-writes** | Writing engine — dissects an idea like a scalpel, peeling layer after layer to the bottom. 1000-1500 words |
| **ljg-invest** | Investment analysis — the core judgment is whether a project is an "order-creating machine" (秩序创造机器) |
| **ljg-read** | Reading companion — accompanies you through any text, with three-layer English translation (faithfulness/fluency/elegance, 信达雅) + structural annotation + deep questioning + cross-domain digressions |
| **ljg-relationship** | Relationship analysis — five-layer structural diagnosis + psychoanalysis, guiding the user through dialogue to help them "see" the true structure of a relationship |
| **ljg-roundtable** | Roundtable discussion — one topic, one roundtable: real figures clash round by round, each round wraps with an ASCII structure diagram, and the full transcript is archived when it adjourns |
| **ljg-travel** | Travel research — input a city name, generates an in-depth cultural research document (org-mode) + a portable card (PNG) |
| **ljg-skill-map** | Skill map — scans all installed skills and renders a visual overview |
| **ljg-present** | Presentation forge — default is Takahashi-style (高桥流) (one keyword per slide, ink text on a cream-white background); `-s` slogan style (VACAT/BIG STUDIOS style: black-and-red color blocks, ultra-bold, full assertive sentences filling the screen)|
| **ljg-push** | Push engine — syncs local `~/.claude/skills/ljg-*` to the GitHub repo in one click (master + md branches)|


## Workflows

Workflows chain multiple skills together into a single command.

| Workflow | Skill Chain | Description |
|--------|--------|------|
| **ljg-paper-flow** | ljg-paper → ljg-library | Reads the paper + casts a viewfinder library card, all in one go |
| **ljg-word-flow** | ljg-word → ljg-card -i | Deep word analysis + infograph card, all in one go |

## Output Formats

Skills are available in two output formats, installed via different branches, with identical functionality:

| Branch | Format | Use Case |
|--------|------|----------|
| `master` (default) | Org-mode (`.org`) | Emacs / Denote users |
| `md` | Markdown (`.md`) | Users in the Markdown ecosystem such as Obsidian / VSCode / Notion |
