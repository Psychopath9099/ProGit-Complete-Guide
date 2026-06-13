---
title: Branching Basics
tags: [git, beginner, branch, switch, checkout]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-branch
related:
  - "[[10 Merging]]"
  - "[[01 Rebase]]"
  - "[[🔀 Workflows Hub]]"
---

# Branching Basics

## Overview

A **branch** is an independent line of development. It's a lightweight pointer to a specific commit. Creating a branch lets you work on a new feature or bug fix in isolation — without touching the main codebase — until you're ready to merge it back.

> [!important] Branches are Git's Killer Feature
> Unlike most VCS tools, Git branches are incredibly cheap. Creating one takes milliseconds and uses almost no disk space. This makes branching-for-everything a natural workflow.

---

## What is a Branch, Really?

At its core, a branch is just a **file containing a 40-character SHA-1 hash** — a pointer to a commit.

```
main  ──→ [commit C]
               ↑
           [commit B]
               ↑
           [commit A]
```

When you create a new branch, Git creates a new pointer at the same commit:

```
main     ──→ [commit C]  ← HEAD
feature  ──→ [commit C]
```

HEAD is a special pointer that tracks *which branch you're currently on*.

When you make a new commit on `feature`:

```
main     ──→ [commit C]
feature  ──→ [commit D] ← HEAD
                 ↑
             [commit C]
```

The branches have diverged. `main` still points to C. `feature` advances to D.

---

## Creating Branches

```bash
# Create a new branch
git branch feature-login

# Create AND switch to it (preferred)
git switch -c feature-login

# Older syntax (still works)
git checkout -b feature-login
```

### Create from a specific commit
```bash
git switch -c hotfix abc1234
git branch hotfix abc1234    # without switching
```

---

## Switching Branches

```bash
# Switch to an existing branch
git switch main
git switch feature-login

# Old syntax (still works)
git checkout main
```

> [!warning] Commit or stash before switching
> Git warns you if switching would overwrite uncommitted changes. Either commit your changes or [[04 Stash|stash]] them before switching branches.

---

## Listing Branches

```bash
# Local branches
git branch

# Output:
#   feature-login
# * main             ← asterisk = current branch
#   hotfix-payment

# Include remote tracking branches
git branch -a

# Verbose (with last commit)
git branch -v
#   feature-login  3a4b5c6 Add login form
# * main           9f8e7d6 Fix payment handler
```

---

## Deleting Branches

```bash
# Delete a merged branch (safe)
git branch -d feature-login

# Force delete (even if unmerged)
git branch -D feature-login
```

> [!note] Remote branches
> Deleting a local branch doesn't delete the remote branch. To delete remote:
> ```bash
> git push origin --delete feature-login
> ```

---

## Remote Branches

When you push a branch to a remote, Git creates a **remote-tracking branch**:

```bash
# Push branch and set tracking
git push -u origin feature-login

# See remote branches
git branch -r
# origin/main
# origin/feature-login
```

Remote tracking branches are like bookmarks: `origin/main` is Git's record of where `main` was on the remote last time you fetched.

### Checking out a remote branch
```bash
git fetch origin
git switch feature-login    # Git auto-tracks origin/feature-login
```

---

## Common Branching Workflows

### Feature Branch (most common)
```bash
# 1. Start from main
git switch main
git pull

# 2. Create a feature branch
git switch -c feature/user-auth

# 3. Work and commit
git add .
git commit -m "Add JWT authentication"
git commit -m "Add login endpoint"

# 4. Push to remote
git push -u origin feature/user-auth

# 5. Open Pull Request on GitHub (or merge locally)
# → See [[10 Merging]] and [[10 Team Workflows]]
```

### Hotfix Branch
```bash
# 1. Branch from main (production)
git switch main
git switch -c hotfix/payment-null-pointer

# 2. Fix and commit
git add .
git commit -m "Fix null pointer in payment handler"

# 3. Merge back to main
git switch main
git merge hotfix/payment-null-pointer

# 4. Clean up
git branch -d hotfix/payment-null-pointer
```

---

## Branch Naming Conventions

Good branch names are descriptive and use slashes as categories:

```
feature/user-authentication
feature/dark-mode
fix/login-redirect-bug
hotfix/critical-payment-error
chore/update-dependencies
docs/api-documentation
refactor/database-layer
```

Avoid: `test`, `fix`, `stuff`, `wip`, `johns-branch`

---

## HEAD Explained

`HEAD` is a special pointer that always points to your current position:

```bash
# See what HEAD points to
cat .git/HEAD
# ref: refs/heads/main   ← on branch main

# After switching
git switch feature
cat .git/HEAD
# ref: refs/heads/feature
```

When HEAD points to a branch, you're on that branch. When HEAD points directly to a commit (not a branch), you're in "detached HEAD" state.

### Detached HEAD
```bash
# Visiting a specific old commit
git checkout abc1234
# "HEAD is now at abc1234... Initial commit"
```

In detached HEAD, you can look around and even make experimental commits — but they won't be saved to any branch. To save your work:
```bash
git switch -c save-my-work   # create a branch at current position
```

---

## Common Mistakes

> [!bug] "I made commits on main instead of a feature branch"
> ```bash
> # Create a branch at current state
> git branch feature/oops
> # Reset main to where it was before your commits
> git reset --hard origin/main
> # Now switch to your branch
> git switch feature/oops
> ```

> [!bug] "I can't delete this branch"
> Use `-D` (capital D) to force delete. Or merge it first, then delete with `-d`.

> [!bug] Accidentally working in detached HEAD
> ```bash
> git switch -c my-rescue-branch   # save work to a named branch
> ```

---

## Exercises

1. Create a branch called `feature/hello`. Add a file. Commit. Switch back to `main`. Notice the file is gone.
2. Create two branches from `main`. Make different commits on each. View the diverged history with `git log --oneline --graph --all`.
3. Practice the feature branch workflow: create → commit → merge → delete.

---

## Related Notes

- [[10 Merging]] — Bringing branches back together
- [[01 Rebase]] — Another way to integrate branches
- [[04 Stash]] — Saving work before switching branches
- [[🔀 Workflows Hub]] — How branches fit into team workflows

---

## Official References

- [`git branch` docs](https://git-scm.com/docs/git-branch)
- [`git switch` docs](https://git-scm.com/docs/git-switch)
- [Pro Git: Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

---

#git/beginner #git/branch #git/switch #git/checkout
