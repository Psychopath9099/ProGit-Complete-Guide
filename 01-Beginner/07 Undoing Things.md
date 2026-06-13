---
title: Undoing Things
tags: [git, beginner, undo, reset, restore, revert]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-reset
related:
  - "[[04 The Three States]]"
  - "[[05 Staging and Committing]]"
  - "[[06 Reflog]]"
---

# Undoing Things

## Overview

One of Git's greatest strengths is that almost everything is undoable. But different situations call for different tools. This note maps every "oops" scenario to the right command.

> [!important] The Key Question
> *When* did the mistake happen?
> - Before staging → `git restore`
> - After staging, before commit → `git restore --staged`
> - After committing (local only) → `git reset`
> - After committing and pushing → `git revert`

---

## The Undo Map

```
┌──────────────────────────────────────────────────────────┐
│  Where is the mistake?         │  How to fix it          │
├────────────────────────────────┼─────────────────────────┤
│  Working tree (not staged)     │  git restore <file>     │
│  Staging area (staged)         │  git restore --staged   │
│  Last commit (local)           │  git commit --amend     │
│  Recent commits (local)        │  git reset              │
│  Pushed commits (shared)       │  git revert             │
└──────────────────────────────────────────────────────────┘
```

---

## Scenario 1: Discard Working Tree Changes

You edited a file and want to throw away those changes (go back to the last commit):

```bash
git restore app.js
```

> [!warning] Destructive — cannot be undone
> This permanently discards your working tree changes. There's no `git undo` for this. Only use it when you're sure.

**Discard ALL working tree changes:**
```bash
git restore .
```

**Old syntax (still works):**
```bash
git checkout -- app.js   # older Git versions
```

---

## Scenario 2: Unstage a File

You staged a file by mistake and want to unstage it (keep changes, just remove from staging):

```bash
git restore --staged app.js
```

The changes stay in your working tree — you haven't lost anything.

**Unstage everything:**
```bash
git restore --staged .
```

**Old syntax:**
```bash
git reset HEAD app.js   # older Git versions
```

---

## Scenario 3: Fix the Last Commit

You just committed and realized you forgot a file, or want to fix the message:

```bash
# Add forgotten file, then amend
git add forgotten-file.js
git commit --amend

# Just fix the message
git commit --amend -m "Better commit message"
```

> [!warning] Only amend local commits
> `--amend` rewrites the last commit (creates a new hash). If you've already pushed it, you'd need a force push, which will cause problems for anyone else who has that commit. See [[06 Reflog]] for advanced recovery.

---

## Scenario 4: `git reset` — Undo Recent Commits

`git reset` moves the current branch pointer to an earlier commit. It has three modes:

### `--soft` — Undo commit(s), keep changes staged

```bash
git reset --soft HEAD~1
```

Before: `[commit A] ← [commit B] ← HEAD`
After:  `[commit A] ← HEAD` (commit B's changes are in the staging area)

**Use case:** You committed too early and want to add more to that commit.

### `--mixed` (default) — Undo commit(s), keep changes unstaged

```bash
git reset HEAD~1        # same as --mixed
git reset --mixed HEAD~1
```

Commit is gone; changes are in your working tree but not staged.

**Use case:** You want to redo the commit from scratch (re-stage, re-organize).

### `--hard` — Undo commit(s), discard changes

```bash
git reset --hard HEAD~1
```

> [!danger] Destructive
> This permanently removes both the commits AND the changes. The files go back to the state of `HEAD~1`. Unless you use `git reflog`, these changes are gone.

**Use case:** You made a mess and just want to go back to a clean state.

---

## Reset Visual

```
Before:  A ← B ← C  ← HEAD (main)
                  ↑
              Your mistake

git reset --soft HEAD~2    → A ← HEAD; B and C's changes are staged
git reset --mixed HEAD~2   → A ← HEAD; B and C's changes are unstaged
git reset --hard HEAD~2    → A ← HEAD; B and C's changes are GONE
```

---

## Scenario 5: `git revert` — Safe Undo for Shared History

When you've already pushed commits and others may have them, use `git revert`. It creates a **new commit** that undoes a previous commit's changes — it doesn't rewrite history.

```bash
# Revert the last commit
git revert HEAD

# Revert a specific commit
git revert abc1234

# Revert a range of commits
git revert HEAD~3..HEAD
```

After `git revert`, push normally:
```bash
git push
```

> [!tip] Revert is always safe
> Because it adds a new commit rather than changing existing ones, it's safe to push to shared branches. Your teammates won't have their history disturbed.

---

## `git revert` vs `git reset`

| | `git reset` | `git revert` |
|---|---|---|
| Rewrites history? | ✅ Yes | ❌ No |
| Creates new commit? | ❌ No | ✅ Yes |
| Safe for shared branches? | ❌ No | ✅ Yes |
| Local use? | ✅ Yes | ✅ Yes |
| Preferred when? | Local cleanup | After pushing |

---

## Recovering from `--hard` Reset (Emergency)

Even after `git reset --hard`, commits aren't immediately deleted — they're orphaned. Use `git reflog` to find them:

```bash
git reflog
# HEAD@{0}: reset: moving to HEAD~2
# HEAD@{1}: commit: The commit I deleted
# HEAD@{2}: commit: Another commit

# Restore it
git reset --hard HEAD@{1}
# or create a branch at that point
git branch recovered-work HEAD@{1}
```

See [[06 Reflog]] for the full rescue guide.

---

## Quick Reference

| Situation | Command |
|---|---|
| Discard file changes (working tree) | `git restore <file>` |
| Unstage a file | `git restore --staged <file>` |
| Unstage everything | `git restore --staged .` |
| Fix last commit message | `git commit --amend -m "new msg"` |
| Undo last commit (keep staged) | `git reset --soft HEAD~1` |
| Undo last commit (keep unstaged) | `git reset HEAD~1` |
| Undo last commit (discard all) | `git reset --hard HEAD~1` |
| Undo a pushed commit safely | `git revert HEAD` |
| Find lost commits | `git reflog` |

---

## Common Mistakes

> [!bug] Used `--hard` when you meant `--soft`
> Check `git reflog` immediately. Your commits should still be there for 90 days.

> [!bug] Reverted the wrong commit
> If you haven't pushed: `git reset HEAD~1` (undoes the revert commit).
> If you've pushed: `git revert HEAD` (undoes the revert by reverting the revert).

---

## Exercises

1. Make a commit, then use `git reset --soft HEAD~1` to unstage it. Add more content, then re-commit.
2. Edit a file, stage it, then use `git restore --staged` to unstage it.
3. Make 3 commits, then use `git reset --hard HEAD~2`. Use `git reflog` to recover.
4. Push a commit, then use `git revert HEAD` to undo it safely.

---

## Related Notes

- [[04 The Three States]] — Understanding what's where
- [[06 Reflog]] — The recovery tool for hard resets
- [[02 Interactive Rebase]] — Rewriting local history more precisely

---

## Official References

- [`git restore` docs](https://git-scm.com/docs/git-restore)
- [`git reset` docs](https://git-scm.com/docs/git-reset)
- [`git revert` docs](https://git-scm.com/docs/git-revert)
- [Pro Git: Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

---

#git/beginner #git/undo #git/reset #git/restore #git/revert
