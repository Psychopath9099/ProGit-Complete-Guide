---
title: Cherry-pick
tags: [git, intermediate, cherry-pick]
type: command-note
difficulty: intermediate
official-doc: https://git-scm.com/docs/git-cherry-pick
related:
  - "[[01 Rebase]]"
  - "[[02 Interactive Rebase]]"
  - "[[06 Reflog]]"
---

# Cherry-pick

## Overview

`git cherry-pick` applies the changes from a specific commit (or range of commits) onto your current branch. Think of it as "copy this commit from over there and apply it here".

---

## When to Use It

- A bug fix was committed to the wrong branch — apply it to the right one
- You need one specific feature commit from a development branch before the whole branch is ready
- Backporting a fix to an older release branch

---

## Basic Syntax

```bash
# Apply a single commit to the current branch
git cherry-pick <commit-hash>

# Apply multiple commits
git cherry-pick abc1234 def5678

# Apply a range of commits (exclusive of first, inclusive of last)
git cherry-pick abc1234..def5678

# Apply range (inclusive of both)
git cherry-pick abc1234^..def5678
```

---

## Practical Example

```bash
# Situation: bug fix on develop, need it on main

git log develop --oneline
# a1b2c3d Fix critical payment bug   ← need this
# e4f5g6h Add new feature (not ready)
# h7i8j9k Experimental change

git switch main

# Apply just the bug fix
git cherry-pick a1b2c3d

# Result: a new commit on main with the same changes
git log main --oneline
# x1y2z3w Fix critical payment bug   ← new commit (different hash)
# ...previous main commits...
```

---

## Cherry-pick Without Committing

```bash
# Apply changes to working tree but don't commit yet
git cherry-pick --no-commit abc1234
# Now you can modify further before committing
git add .
git commit -m "Modified version of the cherry-picked change"
```

---

## Handling Conflicts

If cherry-pick hits a conflict:
```bash
# Resolve conflicts in affected files, then:
git add <resolved-files>
git cherry-pick --continue

# Or bail out
git cherry-pick --abort
```

---

## Notes

- Cherry-pick creates a **new commit** — the original commit stays untouched
- The new commit has a different SHA-1 (different parents, possibly different timestamp)
- If you cherry-pick many commits, consider rebase instead

---

## Related Notes

- [[01 Rebase]] — Move whole branches instead of individual commits
- [[06 Reflog]] — Find commits to cherry-pick

---

## Official References

- [`git cherry-pick` docs](https://git-scm.com/docs/git-cherry-pick)
- [Pro Git: Cherry Picking](https://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project#_rebase_cherry_pick)

---

#git/intermediate #git/cherry-pick
