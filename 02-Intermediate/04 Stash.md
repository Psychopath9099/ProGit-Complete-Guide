---
title: Stash
tags: [git, intermediate, stash]
type: command-note
difficulty: intermediate
official-doc: https://git-scm.com/docs/git-stash
related:
  - "[[09 Branching Basics]]"
  - "[[01 Rebase]]"
  - "[[06 Reflog]]"
---

# Stash

## Overview

`git stash` temporarily shelves changes you've made to your working tree so you can switch context — then re-apply those changes later. It's a "save for later" drawer for work in progress.

**The classic scenario:** You're mid-feature when an urgent bug arrives. You're not ready to commit, but you need a clean working tree to switch branches.

---

## Basic Workflow

```bash
# You have uncommitted changes and need to switch branches

# 1. Stash your work
git stash

# 2. Your working tree is now clean
git status
# nothing to commit, working tree clean

# 3. Switch to another branch and fix the bug
git switch main
git switch -c hotfix/critical-bug
# ... make fix, commit ...

# 4. Come back to your feature
git switch feature/my-feature

# 5. Restore your stashed work
git stash pop
```

---

## All `git stash` Commands

### Save a stash

```bash
# Basic stash (tracked modified files + staged files)
git stash

# With a descriptive message (highly recommended)
git stash push -m "WIP: half-done login form validation"

# Include untracked files too
git stash push -u -m "WIP: login form + new helper file"
git stash push --include-untracked -m "..."

# Include everything including ignored files
git stash push -a -m "full save"

# Stash only specific files
git stash push -m "just the auth changes" src/auth.js tests/auth.test.js
```

### View stashes

```bash
git stash list
# stash@{0}: On feature/login: WIP: half-done login form validation
# stash@{1}: On main: temp save before experiment
# stash@{2}: WIP on main: abc1234 Add README
```

Stashes are numbered from newest (`{0}`) to oldest.

```bash
# See what's in a stash
git stash show stash@{0}
git stash show -p stash@{0}    # with full diff
```

### Apply stashes

```bash
# Apply most recent stash AND remove it from the list
git stash pop

# Apply a specific stash AND remove it
git stash pop stash@{2}

# Apply most recent stash but KEEP it in the list
git stash apply

# Apply a specific stash and keep it
git stash apply stash@{1}
```

> [!tip] `pop` vs `apply`
> Use `pop` when you're done with the stash.
> Use `apply` when you might want to apply it to multiple branches.

### Delete stashes

```bash
# Drop a specific stash
git stash drop stash@{0}

# Clear ALL stashes (careful!)
git stash clear
```

### Create a branch from a stash

```bash
# Create a new branch and apply the stash to it
git stash branch new-branch-name stash@{0}
```

This is the cleanest way to resume stashed work if the base branch has moved on significantly.

---

## Partial Stash with `--patch`

Interactively choose which changes to stash:

```bash
git stash push --patch -m "just some changes"
```

Git shows each hunk — respond `y` to stash, `n` to keep in working tree.

---

## Stash and Conflicts

If applying a stash conflicts with current changes:

```bash
git stash pop
# CONFLICT: ...

# Resolve like a merge conflict, then:
git add <resolved-file>
# The stash is NOT automatically dropped after a conflict
# Drop it manually:
git stash drop stash@{0}
```

---

## When Stash Falls Short

For longer-term "saved states", consider:
- **Committing with `--fixup`** and squashing later with interactive rebase
- **Creating a WIP commit:** `git commit -am "WIP: do not merge"` — then `git reset HEAD~1` when you come back
- **Worktrees** (`git worktree`) — check out multiple branches simultaneously

---

## Common Mistakes

> [!bug] "My stash didn't save new files"
> By default, untracked (new) files are NOT stashed. Use `git stash push -u` to include them.

> [!bug] "I popped and now I have conflicts"
> The stash was saved from a different state of the repo. Resolve conflicts normally, then `git stash drop` to remove the now-applied stash.

> [!bug] Stash list is getting huge
> Review and clean up: `git stash list`, then `git stash drop stash@{n}` for old ones, or `git stash clear` to wipe all.

---

## Quick Reference

| Task | Command |
| ---- | ------- |
| Save work-in-progress | `git stash push -m "description"` |
| Save including new files | `git stash push -u -m "desc"` |
| See all stashes | `git stash list` |
| Restore latest + remove | `git stash pop` |
| Restore latest, keep it | `git stash apply` |
| Restore specific stash | `git stash pop stash@{2}` |
| See stash contents | `git stash show -p stash@{0}` |
| Delete a stash | `git stash drop stash@{0}` |
| Clear all stashes | `git stash clear` |
| New branch from stash | `git stash branch <name> stash@{0}` |

---

## Exercises

1. Make changes to a file, stash them with a message, verify your working tree is clean, then pop them back.
2. Make changes to two separate files. Stash only one of them using `git stash push <filename>`.
3. Create two stashes. Apply them to different branches using `git stash apply`.

---

## Related Notes

- [[09 Branching Basics]] — Switching branches (when you need stash)
- [[02 Interactive Rebase]] — Better for long-term WIP cleanup

---

## Official References

- [`git stash` docs](https://git-scm.com/docs/git-stash)
- [Pro Git: Stashing and Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

---

#git/intermediate #git/stash
