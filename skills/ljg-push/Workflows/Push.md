# Push Workflow

One command to sync ljg-* skills to the github repo (master + md dual branches).

## Voice Notification

```bash
curl -s -X POST http://localhost:31337/notify \
  -H "Content-Type: application/json" \
  -d '{"message": "Running Push in ljg-push"}' \
  > /dev/null 2>&1 &
```

Output text: `Running **Push** in **ljg-push**...`

## Step 0: Pre-push README check (hard gate)

Before every push, ask yourself first:

> Does the README still match the actual set of skills?

Three specific things:

1. *Was a skill added?* → The README's skill list / install commands need a new line
2. *Was a skill deleted?* → The corresponding README line should be removed
3. *Was a skill's description heavily changed?* → The README's summary may need to be synced

The script automatically greps all `ljg-xxx` names in the README and compares them against `~/.codex/skills/ljg-*`. If there's a skill that's local but not in the README, *the push aborts immediately*.

Bypass option (only when you've confirmed the README has already been reviewed and doesn't need updating):

```bash
bash Push.sh --skip-readme-check
```

## Step 1: Parse arguments

| User says | Flag | Effect |
|--------|------|------|
| default | (no flag) | README check + detect changes + dual-branch push |
| "dry-run", "preview it" | `--dry-run` | just list what would be done, don't actually push (README check skipped) |
| "force", "force push" | `--force` | skip detection, force rsync all ljg-* |
| "README already reviewed" | `--skip-readme-check` | skip the README consistency gate (other checks still run) |

## Step 2: Run the script

```bash
bash ~/.codex/skills/ljg-push/Tools/Push.sh [--dry-run|--force]
```

Script logic:

1. *Setup*: check whether `$HOME/code/ljg-skills` exists, clone if not
2. *Detect*: compare `~/.codex/skills/ljg-*` vs `repo/skills/ljg-*`, list the ones that differ
3. *Master push*:
   - `git checkout master` + `git pull --rebase`
   - for each differing skill: `rsync -a --delete --exclude='.git'`
   - bump patch version (plugin.json + marketplace.json)
   - `git add` + `git commit` + `git push origin master`
4. *Md push*:
   - `git checkout md` + `git pull --rebase`
   - for each differing skill: rsync + apply markdown-ization (the `mdize_skill` function — including conversion of the org files themselves: `orgfile_to_md` converts to YAML header/`#` headings then deletes the .org, references are rewritten globally)
   - bump patch version
   - `git add` + `git commit` + `git push origin md`
5. *Wrap-up*: switch back to `master`, leaving the local working repo on the source branch
6. *Report*: list the push results + the diff checklist still needing manual review

## Step 3: Report

Output format:

```
═══ ljg-push Report ═══════════════
Updated skills:
  - ljg-qa
  - ljg-card

master @ v1.17.13 → pushed
md     @ v1.0.8   → pushed

Still needs manual review (diffs not covered by auto-conversion):
  - ljg-xxx/SKILL.md  (`*bold*` markers in body text — italics ambiguity, script leaves it alone)

══════════════════════════════════
```

## Step 4: Exception handling

| Exception | Handling |
|------|------|
| repo path doesn't exist | auto-clone, notify user |
| path exists but isn't an ljg-skills repo | error out, don't destroy the existing directory |
| `git push` rejected by remote (remote has new commits) | try `pull --rebase`, then push again; error out on conflict and let the user resolve it |
| `git pull --rebase` conflict | error out, list conflicting files, suggest `rebase --abort` or manual resolution |
| no changes at all in `~/.codex/skills/ljg-*` | print "Nothing to push." and exit |

## Acceptance criteria

- Both branches have new commits (unless no changes were detected)
- Both remote origin/master and origin/md are updated
- Local `$HOME/code/ljg-skills` ends up resting on `master`
- The report lists version numbers and pushed skills
- Any diffs not covered by markdown-ization are listed in the review checklist
</content>
