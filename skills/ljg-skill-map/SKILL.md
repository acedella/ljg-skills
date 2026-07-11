---
name: ljg-skill-map
description: "Skill map viewer. Scans all installed skills and renders a visual overview — name, version, description, category at a glance. Use when user says 'skills', '技能', '技能地图', 'skill map', '我有哪些技能', '看看技能', '列出技能', 'list skills'. Also trigger when user asks what skills are available or installed."
user_invocable: true
version: "1.0.0"
---

# ljg-skill-map: Skill Map (技能地图)

Scans all installed skills under `~/.claude/skills/` and generates an at-a-glance visual map.

## Execution

### 1. Scan

Run `scripts/scan.sh` to get JSON data for all skills (name, version, invocable, desc).

### 2. Categorize

Based on the skill name and description, automatically classify skills into the following categories:

| Category | Icon | Meaning | Typical Members |
|------|------|------|----------|
| Cognitive Atoms (认知原子) | ◆ | Atomic operations for content processing | ljg-plain, ljg-word, ljg-writes, ljg-paper |
| Output Forging (输出铸造) | ▲ | Transforms content into deliverables | ljg-card |
| Network Reach (联网触达) | ● | Interacts with the external world | agent-reach |
| System Ops (系统运维) | ■ | Maintenance and management of the agent itself | datetime-check, memory-review, save-conversation, skill-creator, ljg-skill-map |
| Environment Setup (环境部署) | ★ | One-time installation and configuration | Her-init |

Classification is based on name prefixes and keywords in the description. When a new skill cannot be classified, place it under "Uncategorized."

### 3. Render

Present it using an ASCII box diagram, in the following format:

```
╔══════════════════════════════════════════════════════════╗
║              SKILL MAP  ·  {N} skills installed         ║
╠══════════════════════════════════════════════════════════╣
║                                                          ║
║  ◆ Cognitive Atoms                                       ║
║  +-----------------+----------------------------------+  ║
║  | ljg-plain v4.0  | Plain — good Qs+analogies to grok |  ║
║  | ljg-word  v1.0  | Deep English word deconstruction  |  ║
║  | ljg-writes v4.0 | Writing engine                    |  ║
║  | ljg-paper v2.0  | Paper reading & analysis          |  ║
║  +-----------------+----------------------------------+  ║
║                                                          ║
║  ▲ Output Forging                                        ║
║  +-----------------+----------------------------------+  ║
║  | ljg-card  v1.5  | Cast — content to PNG visual      |  ║
║  +-----------------+----------------------------------+  ║
║                                                          ║
║  ...                                                     ║
╚══════════════════════════════════════════════════════════╝
```

Rules:
- One block per category, with the category icon + name as the title
- Skill name left-aligned, version number right after it (show `-` if no version)
- Truncate the description to one line, keeping the core meaning
- Skills with user_invocable = true get a `/` marker after the name (indicating it can be invoked directly as `/skill-name`)
- Bottom stats line: total count, invocable count, category count

### 4. Output

Render the ASCII map directly in the conversation. Do not generate files, do not write to disk.
