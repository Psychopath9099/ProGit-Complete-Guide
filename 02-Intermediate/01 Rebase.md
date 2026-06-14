---
title: Rebase
tags: [git, intermediate, rebase, history]
type: command-note
difficulty: intermediate
official-doc: https://git-scm.com/docs/git-rebase
related:
  - "[[02 Interactive Rebase]]"
  - "[[03 Cherry-pick]]"
  - "[[10 Merging]]"
---

# Rebase

## Overview

`git rebase` re-applies commits from one branch onto another. It achieves the same goal as merging — integrating work from one branch into another — but produces a **linear history** instead of creating a merge commit.

---

## The Core Concept

Given this history:
```
      A ← B ← C  (main)
           ↑
           D ← E  (feature)
```

**Merge** creates:
```
      A ← B ← C ← M  (main, merge commit)
           ↑       ↑
           D ← E ──┘
```

**Rebase** replays feature's commits on top of main:
```
      A ← B ← C ← D' ← E'  (feature, rebased)
                   ↑
                  main
```

`D'` and `E'` are new commits with the same content as D and E but different SHA-1 hashes and a new parent (C instead of B).

---

## Basic Rebase

```bash
# From feature branch, rebase onto main
git switch feature
git rebase main
```

What Git does internally:
1. Finds the common ancestor (B)
2. Temporarily stores feature's commits (D, E)
3. Moves feature to point at the tip of main (C)
4. Re-applies each stored commit one by one (creating D', E')

---

## After Rebasing: Update the Remote

After rebasing, the feature branch's history has changed. You need to force-push:

```bash
git push --force-with-lease origin feature
```

> [!tip] Use `--force-with-lease` not `--force`
> `--force` blindly overwrites the remote. `--force-with-lease` fails if someone else has pushed to the branch since your last fetch — safer for teams.

---

## The Golden Rule ⚠️

> [!danger] Never rebase public/shared commits
> **Never rebase commits that have been pushed to a branch others are working on.**
>
> When you rebase, you create new commits with new SHA-1 hashes. If someone else has pulled the original commits, their history is now incompatible with yours. Forcing this on a team causes chaos.
>
> **Safe to rebase:** Your local feature branches before they're shared.
> **Don't rebase:** `main`, `develop`, or any branch others have pulled.

---

## Handling Conflicts During Rebase

Rebasing applies commits one by one, so conflicts appear per-commit (unlike merge which shows all conflicts at once):

```
CONFLICT (content): Merge conflict in src/auth.js
error: could not apply abc1234... Add auth logic
hint: Resolve all conflicts manually, mark them as resolved with
hint: `git add/rm <conflicted_files>`, then run `git rebase --continue`.
```

**Resolution flow:**
```bash
# 1. Edit conflicted files
nano src/auth.js   # resolve conflicts, remove conflict markers

# 2. Stage the resolved file
git add src/auth.js

# 3. Continue to the next commit
git rebase --continue

# Repeat for each conflicting commit
```

**Bail out:**
```bash
git rebase --abort   # return to state before rebase
```

---

## `git pull --rebase`

Instead of merging upstream changes, rebase your local commits on top:

```bash
git pull --rebase

# Equivalent to:
git fetch origin
git rebase origin/main
```

This keeps a cleaner history when updating from a remote.

```bash
# Set as default
git config --global pull.rebase true
```

---

## Rebase onto a Different Branch

```bash
# Move only the commits unique to feature-x onto main
# (not the ones shared with feature-base)
git rebase --onto main feature-base feature-x
```

Useful when you accidentally branched off the wrong branch.

---

## Rebase vs Merge: When to Use Which

| Use Merge When | Use Rebase When |
|---|---|
| Integrating a completed feature to main | Updating a local feature branch with upstream changes |
| On public/shared branches | On your own local branches |
| You want to preserve exact history | You want a clean, linear history |
| Working with non-Git-savvy teammates | You're comfortable with history rewriting |

---

## Practical Workflow

```bash
# Standard feature branch workflow with rebase:

# 1. Start feature
git switch -c feature/payment-v2 main

# 2. Work on it
git commit -m "Add payment model"
git commit -m "Add payment API endpoint"

# 3. Meanwhile, main has been updated. Before merging, rebase:
git fetch origin
git rebase origin/main

# If conflicts: resolve → git add → git rebase --continue

# 4. Now feature is on top of latest main. Push.
git push -u origin feature/payment-v2

# 5. Open PR. When merged, it will look linear.
```

---

## Common Mistakes

> [!bug] "My commits disappeared after rebase"
> They're not gone — check `git reflog`. Rebasing creates new commits; the old ones are still in the reflog for 90 days.
> ```bash
> git reflog
> git reset --hard HEAD@{n}  # restore if needed
> ```

> [!bug] Rebased onto wrong branch
> Use `git rebase --abort` if still in progress. If done, `git reflog` to find the pre-rebase HEAD and `git reset --hard` to it.

---

## Related Notes

- [[02 Interactive Rebase]] — Rewrite history with fine control
- [[10 Merging]] — The simpler alternative
- [[06 Reflog]] — Recovery from rebase mistakes

---

## Official References

- [`git rebase` docs](https://git-scm.com/docs/git-rebase)
- [Pro Git: Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

---

#git/intermediate #git/rebase #git/history
