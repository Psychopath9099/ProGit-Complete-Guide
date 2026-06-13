---
title: Staging and Committing
tags: [git, beginner, add, commit, staging]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-commit
related:
  - "[[04 The Three States]]"
  - "[[06 Viewing History]]"
  - "[[07 Undoing Things]]"
---

# Staging and Committing

## Overview

The two most fundamental Git actions: **staging** selects what goes into your next snapshot, and **committing** saves that snapshot permanently to Git's database. Together they form the core of your daily Git workflow.

---

## `git add` — Staging Changes

`git add` moves changes from the working tree into the staging area (index).

### Syntax
```bash
git add <pathspec>
```

### Common Usage

```bash
# Stage a specific file
git add README.md

# Stage multiple files
git add app.js styles.css

# Stage an entire directory
git add src/

# Stage ALL changes in the current directory
git add .

# Stage all tracked files that have changes (not new files)
git add -u

# Stage everything (new + modified + deleted)
git add -A
```

### Interactive Staging — Choose Specific Lines

This is one of Git's most powerful features:

```bash
git add -p         # or --patch
```

Git shows you each "hunk" (section of changes) and lets you choose per-hunk:
- `y` — stage this hunk
- `n` — skip this hunk
- `s` — split into smaller hunks
- `q` — quit
- `?` — help

**Use case:** You made two unrelated changes in the same file. Stage only one of them to make a clean, focused commit.

---

## `git commit` — Saving a Snapshot

`git commit` takes everything in the staging area and saves it permanently.

### Syntax
```bash
git commit [options]
```

### Common Usage

```bash
# Commit with inline message
git commit -m "Add login button to header"

# Open editor to write a multi-line message
git commit

# Stage all modified tracked files AND commit
git commit -am "Fix typo in README"

# Amend the most recent commit (change message or add files)
git commit --amend
```

### Writing Good Commit Messages

A commit message has a **subject line** and an optional **body**:

```
Short summary (50 chars or less)

More detailed explanation if needed. Wrap at 72 characters.
Explain WHY the change was made, not just what. The diff
shows what changed; the message should explain the reasoning.

- Bullet points are fine for lists
- Use imperative mood: "Fix bug" not "Fixed bug"
```

**Good commit messages:**
```
Add user authentication via JWT
Fix null pointer exception in payment handler
Refactor database connection pooling for performance
Update README with installation instructions
```

**Bad commit messages:**
```
fix
wip
stuff
asdfgh
updated files
```

> [!tip] The 50/72 Rule
> Keep the subject line under 50 characters. Wrap body text at 72 characters. This formats well in `git log`, GitHub, and most tools.

---

## Viewing What You're About to Commit

Before committing, always review:

```bash
# What's staged vs unstaged
git status

# See the actual diff of staged changes
git diff --staged    # (or --cached, same thing)

# See unstaged changes
git diff
```

---

## The `git diff` Command

```bash
# Differences between working tree and staging area (unstaged changes)
git diff

# Differences between staging area and last commit (staged changes)
git diff --staged

# Differences between two commits
git diff abc1234 def5678

# Differences for a specific file
git diff HEAD -- README.md
```

---

## Complete Daily Workflow

```bash
# 1. See what's changed
git status

# 2. Review changes before staging
git diff

# 3. Stage what you want
git add src/feature.js tests/feature.test.js

# 4. Double-check what's staged
git diff --staged

# 5. Commit with a clear message
git commit -m "Add user authentication feature with tests"

# 6. Confirm history
git log --oneline
```

---

## Removing and Moving Files

### Remove a file (stop tracking + delete from disk)
```bash
git rm filename.txt
git commit -m "Remove old config file"
```

### Remove from tracking but keep on disk
```bash
git rm --cached filename.txt
# Now add it to .gitignore
```

### Move/rename a file
```bash
git mv old-name.txt new-name.txt
git commit -m "Rename config file"
```

> [!note] `git mv` vs manual rename
> Renaming a file manually and then doing `git add` works too. Git detects renames based on content similarity. `git mv` is just a convenience shortcut.

---

## Anatomy of a Commit

Every commit stores:
- **Tree** — snapshot of all tracked files
- **Author** — who made the change (from `git config user.name/email`)
- **Committer** — who committed it (usually same as author)
- **Timestamp** — when it was committed
- **Parent** — SHA-1 hash of the previous commit
- **Message** — your description

```bash
git show HEAD    # show the most recent commit in full
```

---

## Common Mistakes

> [!bug] Staging too much with `git add .`
> You accidentally staged files you didn't mean to (like `.env`, `node_modules`, build artifacts). Either unstage them with `git restore --staged <file>` or better, set up [[11 gitignore|`.gitignore`]] first.

> [!bug] Empty commit message
> Git refuses to commit with an empty message. This is intentional — always write a meaningful message.

> [!bug] Committing to wrong branch
> Check `git status` first — it tells you which branch you're on. If you committed to the wrong branch, see [[07 Undoing Things]].

> [!bug] Forgetting to stage new files
> `git commit -am` only stages *modified* tracked files. Brand new files need an explicit `git add`.

---

## Exercises

1. Create 3 files. Stage only 2 of them. Commit. Then stage and commit the third separately.
2. Edit a file. Use `git add -p` to stage only some of the changes.
3. Make a commit, then immediately amend it to fix the message: `git commit --amend -m "Better message"`.

---

## Related Notes

- [[04 The Three States]] — The mental model behind this workflow
- [[06 Viewing History]] — See your commits with `git log`
- [[07 Undoing Things]] — Fix mistakes
- [[11 gitignore]] — Prevent unwanted files from being staged

---

## Official References

- [`git add` docs](https://git-scm.com/docs/git-add)
- [`git commit` docs](https://git-scm.com/docs/git-commit)
- [`git diff` docs](https://git-scm.com/docs/git-diff)
- [Pro Git: Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

---

#git/beginner #git/add #git/commit #git/staging
