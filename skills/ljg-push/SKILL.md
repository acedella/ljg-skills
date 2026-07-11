---
name: ljg-push
description: Sync all updated skills in ~/.codex/skills/ljg-* to the github repo (ljg-skills) — push master branch first (org-mode output style), then switch to the md branch (markdown output style) for basic markdown-ization before pushing. Use when user says '/ljg-push', 'push skills', '推送 skills' (push skills), '同步 skills' (sync skills), 'sync ljg', or whenever ljg-* skills get updated and need shipping. NOT FOR pushing non-ljg skills or arbitrary git repos.
user_invocable: true
---

# ljg-push: Push ljg-* skills

Sync the changed skills in the local `~/.codex/skills/ljg-*` to the github repo with one command, overwriting both the master and md branches.

## Repo paths (hardcoded)

```
SKILLS_REPO="$HOME/code/ljg-skills"     # local working repo
SKILLS_LOCAL="$HOME/.codex/skills"      # local skill source
REPO_URL="git@github.com:lijigang/ljg-skills.git"
```

If `$SKILLS_REPO` doesn't exist, the script will clone it automatically. If it exists but isn't a git repo for ljg-skills, the script will error out (without destroying the existing directory).

## Differences between the two branches

| Branch | Output format | File extension | Bold | File header |
|------|---------|---------|------|--------|
| `master` (default) | org-mode | `.org` | `*bold*` | `#+title:` etc. |
| `md` | markdown | `.md` | `**bold**` | YAML frontmatter |

The skills in `~/.codex/skills/` are in *master style* (the source version). The differences for the md branch are automatically converted by the script, with manual patching where necessary.

After pushing `md`, the script automatically switches back to `master`. The local `$HOME/code/ljg-skills` should always rest on the source branch, for easy viewing and installation next time.

## Workflow

Follow the steps in `Workflows/Push.md` → call `Tools/Push.sh`.

## README consistency (hard gate)

Before every push, the script forces one thing: *cross-check the README against the local skills.*

- List all skill names in `~/.codex/skills/ljg-*`
- Grep the `ljg-xxx` names that appear in `$SKILLS_REPO/README.md`
- Find ones present locally but missing in the README — *this almost certainly means the README wasn't updated*
- If found → abort the push, report the discrepancy

Every push is an opportunity to review the README. Ask yourself:

1. *Was a skill added?* The README's skill list / install commands need a new line
2. *Was a skill deleted?* The corresponding README line should be removed
3. *Was a skill's description heavily changed?* The README's summary may need to be synced

Once you've confirmed the README has been reviewed and truly doesn't need updating, bypass the gate:

```bash
/ljg-push --skip-readme-check
```

## Scope of automatic conversion

Automatically converted during md branch sync (as of 2026-06-12, includes the org files themselves):

- *The org files themselves*: every `.org` file inside a skill (except those in assets/) is converted to a same-named `.md` and the original is deleted — org header block → YAML frontmatter (including `---` fences, `filetags` → `tags`), `*` headings → `#` headings (hierarchy preserved), `#+ATTR_*` lines removed, `[[file:x]]` → `![](x)`, `#+begin_src` → ``` fences. References to the renamed files in other .md files are rewritten globally
- File extension references: `__qa.org` → `__qa.md`, `__paper.org` → `__paper.md` etc. (denote naming convention)
- Keywords: `org-mode` → `markdown`, `Org-mode` → `Markdown`
- Org-style formatting instructions: "bold with *bold* (single asterisk)..." → "bold with **bold** (double asterisk)", "heading levels start from *" → "start from #", "Org file header" → "Markdown file header", 8 example key lines starting with `#+title:` etc. → YAML key lines

*Still not automatically converted* (handle manually as needed):

- `*bold*` markers in the body text: in markdown, `*x*` is italics, so a blind replace would break the document's own formatting
- YAML key lines converted inside SKILL.md example blocks don't carry `---` fences (a known cosmetic gap, doesn't affect semantics)

## Voice Notification

```bash
curl -s -X POST http://localhost:31337/notify \
  -H "Content-Type: application/json" \
  -d '{"message": "Running Push in ljg-push"}' \
  > /dev/null 2>&1 &
```

Output text: `Running **Push** in **ljg-push**...`

## Examples

*Example 1: One-command push*

```
User: /ljg-push
→ Detect skills in ~/.codex/skills/ljg-* that differ from the repo
→ master: rsync + bump version + commit + push
→ md: rsync + mdize + bump version + commit + push
→ switch back to master
→ report: which skills were pushed, new version numbers, remaining manual diffs
```

*Example 2: Preview what would be pushed without actually pushing*

```
User: /ljg-push --dry-run
→ list the skills that would be synced
→ list the markdown-ization conversions that would be done
→ don't execute rsync / commit / push
```

## Gotchas

- *README drift is the easiest thing to overlook* — push right after adding a new skill and the README is still stuck on the old list. The script now has a hard gate to catch this; when it blocks you, don't blindly add `--skip-readme-check` — go check the README first
- *The script assumes git credentials are already set up* (ssh key or PAT) — ljg-push doesn't handle authentication; it errors out directly on auth failure
- *master must be pushed first* — the md branch's markdown-ization is converted based on master's org version. Pushing in reverse order breaks the sequence
- *Untracked clutter (like `assets/measure.js`) will get synced into the repo by rsync* — delete it locally first if you don't want it pushed, or add it to `.gitignore`
- *The org files themselves are now auto-converted (as of 2026-06-12)* — files like template.org get converted to .md and the originals deleted, regenerated on every push (rsync --delete overwriting them is harmless, it's idempotent). The only remaining manual item is the `*bold*` marker in body text. After adding a new org reference file with complex constructs, run `--dry-run` or test in a sandbox first to check the mdize conversion result
- *The script auto-bumps the patch version in plugin.json + marketplace.json* — if you want to bump minor / major, do it manually first before running the script; the script only appends a patch bump
- *If the md branch's remote is ahead of local* (e.g. pushed from another machine just now), the script will try `pull --rebase`, and if that fails, attempt a `reset --hard origin/md` and reapply — this discards any unpushed local md branch commits. The script warns before doing this
- *Migration note*: the repo history once lived at `~/.claude.backup-20260502/ljg-skills-repo/` (the "backup" in the path name is a historical leftover), moved to `~/code/ljg-skills/` on 2026-05-02
</content>
