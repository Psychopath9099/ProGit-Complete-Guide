---
title: Interactive Rebase
tags: [git, intermediate, rebase, history, squash]
type: command-note
difficulty: intermediate
official-doc: https://git-scm.com/docs/git-rebase
related:
  - "[[01 Rebase]]"
  - "[[03 Cherry-pick]]"
  - "[[06 Reflog]]"
---

# Interactive Rebase

## Overview

`git rebase -i` (interactive rebase) lets you **rewrite your commit history** — squash commits, reorder them, reword messages, split commits, and drop them entirely. It's a pre-push cleanup tool.

> [!tip] Use it to make your history tell a clear story
> You might work in messy commits ("wip", "fix", "typo") and clean them up into logical, professional commits before pushing.

---

## Basic Syntax

```bash
# Interactively rebase the last N commits
git rebase -i HEAD~N

# Interactively rebase everything after a specific commit
git rebase -i abc1234

# Interactively rebase since branching from main
git rebase -i main
```

---

## The Interactive Rebase Editor

Running `git rebase -i HEAD~4` opens your editor:

```
pick a1b2c3d Add login form
pick e4f5g6h Add form validation
pick h7i8j9k Fix typo in form
pick l0m1n2o Add login tests

# Rebase b9c8d7e..l0m1n2o onto b9c8d7e (4 commands)
#
# Commands:
# p, pick   = use commit
# r, reword = use commit, but edit the commit message
# e, edit   = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup  = like squash, but discard this commit's message
# d, drop   = remove commit
# b, break  = stop here (continue with 'git rebase --continue')
```

You edit this list and save. Git replays the commits in order based on your instructions.

---

## The Commands

### `pick` — Keep a commit as-is
```
pick a1b2c3d Add login form
```

### `reword` (r) — Keep the commit but change the message
```
reword a1b2c3d Add login form
```
Git will pause and open your editor for each `reword`.

### `squash` (s) — Merge into previous commit, combine messages
```
pick a1b2c3d Add login form
squash e4f5g6h Add form validation
squash h7i8j9k Fix typo in form
```
Result: one commit with a combined message from all three. Git opens editor to let you write the final message.

### `fixup` (f) — Merge into previous, discard this commit's message
```
pick a1b2c3d Add login form
fixup e4f5g6h Add form validation
fixup h7i8j9k Fix typo in form
```
Result: one clean commit with only "Add login form" message. Best for "oops" commits.

### `drop` (d) — Completely remove a commit
```
drop h7i8j9k Add debugging console.log
```

### `edit` (e) — Pause to amend a commit
```
edit a1b2c3d Add login form
```
Git pauses at this commit. You can:
- `git commit --amend` to change the commit
- Or even `git add / git commit` to split it into multiple commits
- Then `git rebase --continue` to proceed

### Reorder commits
Simply move the lines around:
```
pick l0m1n2o Add login tests     ← moved to top
pick a1b2c3d Add login form
pick e4f5g6h Add form validation
```

---

## Practical Examples

### Clean up messy WIP commits before PR
```
# Before:
pick a1b2 Start login feature
pick c3d4 wip
pick e5f6 fix
pick g7h8 more fixing
pick i9j0 final fix finally

# After (in the editor):
pick a1b2 Start login feature
fixup c3d4 wip
fixup e5f6 fix
fixup g7h8 more fixing
fixup i9j0 final fix finally
```

Result: One clean commit "Start login feature" with all the changes.

### Fix a commit message deep in history
```
reword abc123 Bad old message
```

### Remove a debugging commit
```
pick a1b2c3 Add feature
drop d4e5f6 DEBUG: remove before push
pick g7h8i9 Add tests
```

---

## Autosquash — Even Faster Cleanup

If you name your commits with `fixup!` or `squash!`, Git can automatically arrange them:

```bash
# Make a fixup commit that targets "Add login form"
git commit --fixup a1b2c3d
# Creates commit: "fixup! Add login form"

# Then auto-squash
git rebase -i --autosquash HEAD~4
# Git automatically moves and marks the fixup commit
```

---

## Splitting a Commit

If a commit does too much, split it:

1. In the rebase editor, mark the commit as `edit`
2. When Git pauses:
```bash
# Undo the commit but keep changes
git reset HEAD~1

# Now stage and commit in pieces
git add src/auth.js
git commit -m "Add auth logic"

git add tests/auth.test.js
git commit -m "Add auth tests"

# Continue
git rebase --continue
```

---

## After Interactive Rebase

If the branch was already pushed, you need to force-push:
```bash
git push --force-with-lease origin feature-branch
```

> [!warning] Only on your own feature branches
> Never interactively rebase shared branches.

---

## Common Mistakes

> [!bug] "I saved the editor by mistake with wrong instructions"
> ```bash
> git rebase --abort   # Immediately abort if things go wrong
> ```

> [!bug] "Conflicts during interactive rebase"
> Same as regular rebase: resolve → `git add` → `git rebase --continue`.

---

## Related Notes

- [[01 Rebase]] — Regular rebase foundation
- [[03 Cherry-pick]] — Applying individual commits
- [[06 Reflog]] — Recovery if something goes wrong

---

## Official References

- [`git rebase -i` docs](https://git-scm.com/docs/git-rebase)
- [Pro Git: Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

---

#git/intermediate #git/rebase #git/history #git/squash
