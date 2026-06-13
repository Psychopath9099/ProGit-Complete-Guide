---
title: Viewing History
tags: [git, beginner, log, history, diff]
type: command-note
difficulty: beginner
official-doc: https://git-scm.com/docs/git-log
related:
  - "[[05 Staging and Committing]]"
  - "[[07 Undoing Things]]"
  - "[[09 Branching Basics]]"
---

# Viewing History

## Overview

`git log` shows you the commit history of your project. It's how you answer: *What changed? When? Who did it? Why?* Mastering `git log` and `git diff` makes you a much more effective developer and debugger.

---

## `git log` — The Basic Log

```bash
git log
```

Output:
```
commit 3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b
Author: Jane Dev <jane@example.com>
Date:   Mon Jan 13 14:32:01 2025 +0530

    Add user authentication with JWT

commit 9f8e7d6c5b4a3f2e1d0c9b8a7f6e5d4c3b2a1f0e
Author: Jane Dev <jane@example.com>
Date:   Sun Jan 12 10:15:44 2025 +0530

    Fix null pointer in payment handler
```

Each entry shows the commit hash, author, date, and message.

---

## Useful `git log` Flags

### `--oneline` — One line per commit
```bash
git log --oneline
# 3a4b5c6 Add user authentication with JWT
# 9f8e7d6 Fix null pointer in payment handler
# 1a2b3c4 Initial commit
```

### `--graph` — Visual branch structure
```bash
git log --oneline --graph
# * 3a4b5c6 Add user authentication
# * 9f8e7d6 Fix null pointer
# | * f1e2d3c (feature) Experimental dark mode
# |/
# * 1a2b3c4 Initial commit
```

### `--all` — Show all branches (not just current)
```bash
git log --oneline --graph --all
```

### Combined (save as an alias)
```bash
git log --oneline --graph --all --decorate
```

> [!tip] Save as alias
> ```bash
> git config --global alias.lg "log --oneline --graph --all --decorate"
> ```
> Now just type `git lg`.

### `-n` — Limit to N commits
```bash
git log -5              # last 5 commits
git log --oneline -10   # last 10, one per line
```

### `--since` / `--until` — Filter by date
```bash
git log --since="2 weeks ago"
git log --since="2025-01-01" --until="2025-02-01"
```

### `--author` — Filter by author
```bash
git log --author="Jane"
git log --author="jane@example.com"
```

### `--grep` — Search commit messages
```bash
git log --grep="authentication"
git log --grep="fix" --oneline
```

### `-S` — Search for changes to a string (pickaxe)
```bash
# Find commits that added or removed this string
git log -S "getUserById"
```

This is incredibly useful for finding when a specific function was introduced or removed.

### `-p` — Show the actual diff for each commit
```bash
git log -p
git log -p -2   # last 2 commits with diffs
```

### `--stat` — Summary of changes per file
```bash
git log --stat
# 3a4b5c6 Add user authentication
#  src/auth.js | 45 ++++++++++++++++++++++++
#  tests/auth.test.js | 32 ++++++++++++++++
#  2 files changed, 77 insertions(+)
```

---

## Inspecting a Single Commit

```bash
# Show a specific commit's full diff
git show 3a4b5c6

# Show just the metadata (no diff)
git show 3a4b5c6 --stat

# Show the most recent commit
git show HEAD

# Show 3 commits ago
git show HEAD~3
```

---

## Comparing Commits and Branches

```bash
# Changes between two commits
git diff abc1234..def5678

# Changes on feature branch not in main
git diff main..feature-branch

# What changed in the last 3 commits
git diff HEAD~3..HEAD

# Changes to a specific file over time
git diff HEAD~5 HEAD -- src/app.js
```

---

## `git blame` — Who Wrote This Line?

```bash
git blame src/auth.js
```

Output:
```
3a4b5c6d (Jane Dev 2025-01-13 14:32:01 +0530  1) const jwt = require('jsonwebtoken');
9f8e7d6c (John Dev 2025-01-12 10:15:44 +0530  2) const User = require('./models/user');
```

Shows every line, who last modified it, and which commit.

```bash
# Blame a specific range of lines
git blame -L 10,20 src/auth.js
```

---

## Finding Changes to a File

```bash
# Full history of changes to one file
git log --follow -p src/auth.js

# When was a file added?
git log --diff-filter=A -- filename.txt

# Who deleted a file?
git log --diff-filter=D -- old-file.txt
```

---

## Referencing Commits

Git has a powerful way to reference commits relative to others:

| Reference | Meaning |
|---|---|
| `HEAD` | Current commit (tip of current branch) |
| `HEAD~1` or `HEAD~` | One commit before HEAD |
| `HEAD~3` | Three commits before HEAD |
| `HEAD^` | Parent of HEAD (same as `HEAD~1`) |
| `HEAD^^` | Grandparent of HEAD |
| `abc1234` | Specific commit (can use short SHA) |
| `main` | Tip of the main branch |
| `v1.0` | A tag named v1.0 |

---

## Practical Debugging Workflow

```bash
# 1. Something broke. When did it work?
git log --oneline   # scroll through history

# 2. Found suspicious commits? See what changed:
git show abc1234

# 3. Not sure which commit broke it? Use bisect:
# → See [[07 Bisect]]

# 4. Something went wrong with a specific function:
git log -S "functionName" --oneline

# 5. See full timeline of a file:
git log --follow -p src/important-file.js
```

---

## Common Mistakes

> [!bug] Can't find a commit
> Check `git log --all` — the commit might be on a different branch. Or use `git reflog` to find lost commits.

> [!bug] `git log` shows nothing
> You have no commits yet. Make at least one with `git commit`.

---

## Exercises

1. Make 5 commits in a repo. Try every variant of `git log` in this note.
2. Use `git log --grep` to find a specific commit by keyword.
3. Use `git log -S` to find when a specific function was first added.
4. Use `git blame` to see who wrote each line of a file.

---

## Related Notes

- [[05 Staging and Committing]] — Creating the history
- [[07 Undoing Things]] — Going back in history
- [[07 Bisect]] — Finding which commit introduced a bug

---

## Official References

- [`git log` docs](https://git-scm.com/docs/git-log)
- [`git diff` docs](https://git-scm.com/docs/git-diff)
- [`git blame` docs](https://git-scm.com/docs/git-blame)
- [Pro Git: Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

---

#git/beginner #git/log #git/history #git/diff
