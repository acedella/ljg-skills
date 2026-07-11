# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is lijigang's personal Claude Code skills repository, packaged as a Claude Code plugin. Each skill is a self-contained directory under `skills/` that extends Claude Code's capabilities.

Important: this repo is a **sync target**, not the primary authoring location. Skills are authored in the local skills directory (`~/.claude/skills/`) and shipped here via the `ljg-push` skill / `scripts/sync-push.sh`. Direct edits to `skills/` in this repo will be overwritten by the next sync unless also applied upstream.

## Repository Structure

```
ljg-skills/
├── skills/             # All skills, each with "ljg-" prefix
│   └── ljg-*/
│       ├── SKILL.md    # Skill definition with YAML frontmatter
│       ├── references/ # Reference docs for complex skills
│       ├── assets/     # Templates, images, scripts
│       └── scripts/    # Helper scripts (bash, node)
├── scripts/
│   ├── install.sh      # Installs ljg-card's npm + Playwright deps
│   └── sync-push.sh    # Syncs one skill from ~/.claude/skills/, bumps version, pushes
├── .claude-plugin/     # Plugin metadata (plugin.json, marketplace.json)
├── CLAUDE.md
└── README.md
```

## Branch Model

- `master` — canonical branch; skill output style is org-mode
- `md` — same skills converted to markdown output style (for Obsidian / VSCode / Notion users)

Changes land on `master` first; `ljg-push` then converts and pushes `md`. Commit messages follow `feat: sync ljg-* skills [names] (vX.Y.Z)`, where the version matches `.claude-plugin/plugin.json` (auto-bumped by `sync-push.sh`).

## Skill Format

Each `SKILL.md` follows this structure:

```yaml
---
name: skill-name
description: "What this skill does. Use when user says..."
user_invocable: true|false
version: "x.x.x"
---

# Skill content in markdown...
```

## Skill Inventory

### Thinking tools
| Skill | Purpose |
|-------|---------|
| `ljg-think` | Drill one idea vertically to its irreducible root (追本) |
| `ljg-rank` | Reduce a domain to its minimal independent generators (降秩) |
| `ljg-constraint` | Map the constraints that frame a domain (world/rules/interpretation) |
| `ljg-learn` | Dissect a concept through 8 dimensions into an epiphany |
| `ljg-blind` | Scan yesterday's AI conversations for cognitive blind spots + WeRead chapter |
| `ljg-qa` | Extract a text's reasoning as a chain of incisive Q-A pairs |
| `ljg-roundtable` | Multi-persona structured debate on a topic |
| `ljg-relationship` | Relationship diagnostics (5-layer structure + psychoanalytic depth) |

### Reading pipeline
| Skill | Purpose |
|-------|---------|
| `ljg-paper` | Papers as world-model updates for non-academics |
| `ljg-paper-river` | Trace a paper's citation lineage backward, narrate problem evolution |
| `ljg-book` | Book deconstruction: what problem, what insight, how to use it |
| `ljg-read` | Reading companion with translation and guided questioning |
| `ljg-word` | Deep-dive on a single English word |

### Writing / expression
| Skill | Purpose |
|-------|---------|
| `ljg-writes` | Essay engine: scalpel-style pieces, ~1000-1500 chars |
| `ljg-plain` | Rewrite anything so a smart 12-year-old groks it (白) |

### Visual output
| Skill | Purpose | External Dependencies |
|-------|---------|----------------------|
| `ljg-card` | Content → PNG via 7 molds (long, infograph, multi, sketchnote, comic, whiteboard, big-font) | Node.js + Playwright |
| `ljg-library` | Book → framing-frame (取景框) 2050 library card PNG | AI image generation |
| `ljg-map` | Industry → ecological terrain map PNG | AI image generation |
| `ljg-present` | Org/markdown outline → single-file HTML slides | None |

### Composite workflows
| Skill | Purpose |
|-------|---------|
| `ljg-paper-flow` | ljg-paper + ljg-library card per paper |
| `ljg-word-flow` | ljg-word + ljg-card infograph per word |
| `ljg-travel` | City → museum/architecture research doc + reference cards |

### Domain analysis
| Skill | Purpose |
|-------|---------|
| `ljg-invest` | Investment report judged on one question: is it an order-creating machine (秩序创造机器)? |

### Meta / ops
| Skill | Purpose |
|-------|---------|
| `ljg-skill-map` | Visual overview of installed skills |
| `ljg-push` | Sync updated skills from local authoring dir to this repo (master + md branches) |

## Commands

### Install ljg-card Dependencies

`ljg-card` requires Playwright for screenshot capture:

```bash
bash scripts/install.sh
# or manually:
cd skills/ljg-card && npm install && npx playwright install chromium
```

### Sync a Skill into the Repo

```bash
scripts/sync-push.sh <skill-name> <commit-message>
```

Rsyncs `~/.claude/skills/<skill-name>/` into `skills/`, auto-bumps the patch version in `.claude-plugin/{plugin,marketplace}.json`, commits, and pushes.

### Test ljg-skill-map Scanner

```bash
bash skills/ljg-skill-map/scripts/scan.sh
```

### Install Skills (for users)

```bash
# All skills, org-mode format
npx skills add lijigang/ljg-skills -g --all

# Markdown format (md branch)
npx skills add lijigang/ljg-skills#md -g --all

# Or copy manually
cp -r skills/ljg-* ~/.claude/skills/
```

## Architecture Notes

### Skill Invocation

- Skills with `user_invocable: true` can be triggered via `/skill-name` or natural language
- Trigger phrases are defined in each skill's `description` field, in both English and Chinese (e.g. '拆书', '降秩', '铸') — keep both when editing descriptions
- Descriptions double as routers: adjacent thinking skills carry explicit "NOT FOR" clauses redirecting to each other
- Skills can call other skills via the Skill tool

### Content Processing Pipeline

Several skills share a common pattern for content ingestion:
- **URL** → WebFetch
- **File path** → Read tool
- **Raw text** → Direct use

### ljg-card Architecture

The most complex skill, with multiple rendering modes:

1. **HTML Templates**: Stored in `assets/` (one per mold)
2. **Capture Script**: `assets/capture.js` uses Playwright to screenshot HTML → PNG
3. **Reference Docs**: `references/taste.md` (design guidelines), `references/mode-*.md` (mode-specific instructions)
4. **Output**: PNG files written to `~/Downloads/`

Several visual skills (`ljg-library`, `ljg-map`) share a consistent art identity: Seiji Yoshida (吉田诚治) picture-book style with Jigang (继刚) as recurring character.

### Shared Conventions

**Language**: Skill instructions are written in English; Chinese trigger phrases are retained in descriptions. Skills produce output in the same language as the user's input/request.

**Org-mode output** (ljg-paper, ljg-plain, ljg-writes, and other note-producing skills):
- Bold: `*text*` (single asterisk, not `**`)
- Filenames: `{timestamp}--{title}__{type}.org`
- Output directory: `~/Documents/notes/`
- Timestamps: `date +%Y%m%dT%H%M%S`

**ASCII Art**:
- Allowed: `+ - | / \ > < v ^ * = ~ . : # [ ] ( ) _ , ; ! ' "`
- Forbidden: Unicode box-drawing characters

## Development Guidelines

- Skills are atomic units—each skill directory is self-contained
- Version numbers are manually maintained in SKILL.md frontmatter; the plugin-level version in `.claude-plugin/` is auto-bumped by `sync-push.sh`
- The `.gitignore` ignores all files by default; explicitly unignore with `!pattern`
- When modifying skill logic, update both the SKILL.md and any referenced files in `references/`
- Prefer editing skills in `~/.claude/skills/` and syncing here, so the next sync doesn't clobber repo-only changes

## Testing Changes

After modifying a skill:
1. Copy to `~/.claude/skills/`
2. Restart Claude Code to reload skills
3. Test via natural language trigger or `/skill-name`
