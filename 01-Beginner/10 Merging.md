---
title: Merging
tags: [git, beginner, merge, conflict]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-merge
related:
  - "[[09 Branching Basics]]"
  - "[[01 Rebase]]"
  - "[[🔧 Troubleshooting Hub]]"
---

# Merging

## Overview

**Merging** brings the work from one branch into another. It's how you complete a feature branch and integrate it back into your main codebase. Git has two merge strategies: **fast-forward** and **three-way merge**.

---

## Basic Merge Syntax

```bash
# Switch to the branch you want to merge INTO
git switch main

# Merge the branch you want to bring in
git merge feature/user-auth
```

---

## Fast-Forward Merge

A **fast-forward** merge happens when the target branch hasn't diverged — Git simply moves the pointer forward.

**Before:**
```
main     A ← B ← C
                  ↑
feature           D ← E ← HEAD
```

**After `git merge feature`:**
```
main     A ← B ← C ← D ← E ← HEAD
```

No merge commit is created. History looks completely linear.

```bash
git switch main
git merge feature
# "Fast-forward"
# main is now at commit E
```

---

## Three-Way Merge

A **three-way merge** happens when both branches have diverged — each has commits the other doesn't. Git creates a special **merge commit** that has two parents.

**Before:**
```
         A ← B ← C ← F  (main)
                  ↑
                  D ← E  (feature)
```

**After:**
```
         A ← B ← C ← F ← M  (main, HEAD)
                  ↑       ↑
                  D ← E ──┘  (merge commit M has two parents: F and E)
```

```bash
git switch main
git merge feature
# Merge branch 'feature' into main
# [main abc1234] Merge branch 'feature'
```

---

## Merge Commit Message

When a merge commit is created, Git opens your editor for the merge message. The default is fine. Just save and close:
```
Merge branch 'feature/user-auth'
```

Or skip the editor:
```bash
git merge feature/user-auth -m "Merge user auth feature"
```

---

## `--no-ff` — Always Create a Merge Commit

Even if a fast-forward is possible, you can force a merge commit:

```bash
git merge --no-ff feature/user-auth
```

**Why?** It preserves in the history that a feature branch existed. Some teams enforce this for clarity.

---

## Merge Conflicts

A **conflict** occurs when the same part of the same file was changed differently on both branches. Git can't decide which version to keep — it asks you.

```
CONFLICT (content): Merge conflict in src/auth.js
Automatic merge failed; fix conflicts and then commit the result.
```

### What a conflict looks like inside a file

```
<<<<<<< HEAD
const timeout = 5000;
=======
const timeout = 10000;
>>>>>>> feature/user-auth
```

- `<<<<<<< HEAD` — your current branch's version
- `=======` — separator
- `>>>>>>> feature/user-auth` — the incoming branch's version

### Resolving a conflict

1. **Open the conflicted file** in your editor
2. **Choose what to keep** — edit the file to what it should be (remove all conflict markers)
3. **Stage the resolved file**
4. **Complete the merge with a commit**

```bash
# After editing the file to resolve it:
git add src/auth.js

# Complete the merge
git commit    # Git auto-fills a merge message
```

### Aborting a merge

If you're overwhelmed by conflicts and want to start over:

```bash
git merge --abort
```

This returns everything to the state before you started the merge.

---

## Conflict Resolution Strategies

### Manual (always works)
Open the file, choose what to keep, delete conflict markers.

### Using a visual merge tool
```bash
git mergetool
# Opens your configured visual tool (VS Code, vimdiff, etc.)
```

Set your preferred tool:
```bash
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

### Accept all "ours" or "theirs"

```bash
# Keep all YOUR version for a file
git checkout --ours src/auth.js
git add src/auth.js

# Keep all THEIR version for a file
git checkout --theirs src/auth.js
git add src/auth.js
```

---

## Practical Full Workflow

```bash
# 1. Finish your feature
git switch feature/user-auth
git add .
git commit -m "Finalize user auth"

# 2. Update main before merging
git switch main
git pull   # get latest from remote

# 3. Merge the feature
git merge feature/user-auth

# 4a. If clean merge:
git push

# 4b. If conflicts:
# → Edit conflicted files
# → git add <resolved files>
# → git commit
# → git push

# 5. Clean up
git branch -d feature/user-auth
git push origin --delete feature/user-auth
```

---

## Merge vs Rebase

| | Merge | Rebase |
|---|---|---|
| Creates merge commit? | ✅ (three-way) | ❌ |
| Preserves exact history? | ✅ | ❌ (rewrites) |
| Linear history? | ❌ | ✅ |
| Safe for shared branches? | ✅ | ⚠️ (only local) |
| Beginner-friendly? | ✅ | ⚠️ |

> [!tip] Start with merge
> Merging is the safe default. Learn rebase once you're comfortable with the basics. See [[01 Rebase]].

---

## Common Mistakes

> [!bug] "I merged the wrong branch"
> If you haven't pushed: `git reset --hard HEAD~1` (removes the merge commit).
> If it was a fast-forward, the reset also works.

> [!bug] "There are 50 conflicts and I don't know what to do"
> ```bash
> git merge --abort   # Start over
> ```
> Then consider rebasing your feature branch on main first to resolve conflicts one commit at a time.

> [!bug] "The merge commit clutters my history"
> Try `git merge --squash` to squash all feature commits into one, or use `git rebase` instead.

---

## Exercises

1. Create two branches from `main`. Add different files on each. Merge both into `main`.
2. Create a conflict: edit the same line in the same file on two branches. Merge and resolve the conflict.
3. Try `git merge --abort` mid-conflict to see how recovery works.

---

## Related Notes

- [[09 Branching Basics]] — Creating and switching branches
- [[01 Rebase]] — Alternative to merge for clean history
- [[🔧 Troubleshooting Hub]] — More conflict resolution help

---

## Official References

- [`git merge` docs](https://git-scm.com/docs/git-merge)
- [Pro Git: Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

---

#git/beginner #git/merge #git/conflict
