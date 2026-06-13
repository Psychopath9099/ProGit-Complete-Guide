---
title: The Three States
tags: [git, beginner, concepts, core]
type: concept-note
difficulty: beginner
official-doc: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F
related:
  - "[[05 Staging and Committing]]"
  - "[[06 Viewing History]]"
  - "[[07 Undoing Things]]"
---

# The Three States

## Overview

> [!important] This is the single most important concept in Git.
> Everything else becomes much easier once you truly understand the Three States. Re-read this note until it clicks.

Every file in a Git repository lives in exactly one of **three states**:

| State | Meaning |
|---|---|
| **Modified** | You've changed the file but haven't told Git about it yet |
| **Staged** | You've marked the change to be included in the next commit |
| **Committed** | The change is safely saved in Git's database |

---

## The Three Sections of a Git Project

These three states correspond to three physical locations:

```
┌─────────────────────────────────────────────────────────────┐
│                      Your Project                           │
│                                                             │
│  ┌──────────────┐   git add   ┌──────────────┐             │
│  │   Working    │ ──────────→ │   Staging    │             │
│  │     Tree     │             │     Area     │             │
│  │  (your disk) │ ←────────── │   (index)    │             │
│  └──────────────┘  git restore└──────┬───────┘             │
│                                      │ git commit           │
│                                      ↓                      │
│                           ┌──────────────────┐             │
│                           │   Git Directory  │             │
│                           │   (.git folder)  │             │
│                           │  (the database)  │             │
│                           └──────────────────┘             │
└─────────────────────────────────────────────────────────────┘
```

### Working Tree
The actual files on your disk — what you see in your editor. When you edit a file, it becomes **modified**.

### Staging Area (Index)
A preparation zone. When you `git add` a file, you're copying its current snapshot into the staging area. Only staged changes go into the next commit.

The staging area lets you craft precise commits. You can modify 5 files but only commit 2 of them.

### Git Directory (`.git/`)
The permanent database. When you `git commit`, Git takes everything in the staging area and stores it as a snapshot. This is your history.

---

## The Basic Workflow

```
1. Edit files in your Working Tree
         ↓
2. git add <file>   →  Staging Area
         ↓
3. git commit -m "message"   →  Git Directory (permanent)
```

In code:
```bash
# Make a change
echo "new feature" >> app.js

# Check status (see it's modified)
git status
# Changes not staged for commit:
#     modified:   app.js

# Stage it
git add app.js

# Check status again (see it's staged)
git status
# Changes to be committed:
#     modified:   app.js

# Commit it (now it's permanent)
git commit -m "Add new feature to app.js"

# Check status (clean)
git status
# nothing to commit, working tree clean
```

---

## Why the Staging Area Exists

The staging area might seem like an unnecessary extra step. It exists to give you **precision**.

**Scenario:** You've edited 4 files today:
- `feature.js` — new feature (ready to commit)
- `bugfix.js` — critical bug fix (ready to commit)
- `experiment.js` — half-finished experiment (NOT ready)
- `notes.txt` — personal notes (never commit this)

Without a staging area, you'd have to commit all 4 or none. With staging:

```bash
git add feature.js bugfix.js
git commit -m "Add feature and fix critical bug"
# experiment.js and notes.txt are left unstaged
```

Now your history is clean and logical.

---

## File Lifecycle Diagram

```
Untracked → Staged → Committed
              ↓           ↓
           Modified → Staged again
```

Full lifecycle as described in Pro Git:

```
Untracked    Unmodified    Modified    Staged
    │              │           │          │
    │── git add ──────────────────────→  │
    │              │           │          │──── git commit ────→ Unmodified
    │              │── edit ──→│          │
    │              │           │── git add ──→ │
    │              │── git rm ──→ Untracked    │
```

---

## Checking the State of Files

`git status` is your most important command:

```bash
git status

# Example output:
On branch main
Changes to be committed:            ← STAGED
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md

Changes not staged for commit:      ← MODIFIED
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   app.js

Untracked files:                    ← NOT TRACKED AT ALL
  (use "git add <file>..." to include in what will be committed)
        newfile.txt
```

### Short Status
```bash
git status -s   # or --short

# Output:
 M app.js      ← modified, not staged (right column = working tree)
M  README.md   ← modified and staged (left column = staging area)
?? newfile.txt ← untracked
```

---

## Moving Between States

| Action | Command | What it does |
|---|---|---|
| Working → Staged | `git add <file>` | Stage changes |
| Staged → Committed | `git commit` | Permanently save |
| Staged → Working | `git restore --staged <file>` | Unstage (keep changes) |
| Working → Last Commit | `git restore <file>` | Discard changes ⚠️ |

> [!warning] `git restore <file>` is destructive
> This discards your changes in the working tree. Unlike most Git operations, this cannot be undone. Only use it when you're sure you want to throw away those changes.

---

## The Shortcut: Skip Staging

You can skip the staging area for *already-tracked* files using `-a`:

```bash
git commit -a -m "message"
# or
git commit -am "message"
```

This stages all modifications to tracked files and commits immediately. Note: it does **not** include brand new (untracked) files.

---

## Common Mistakes

> [!bug] "I staged the wrong file"
> ```bash
> git restore --staged <file>    # unstage it (changes preserved)
> ```

> [!bug] "I edited the file AFTER staging it"
> The staging area captures a snapshot at the moment you ran `git add`. If you edit the file again after staging, you'll see it appears in both "staged" and "unstaged" sections of `git status`. Run `git add` again to re-stage the latest version.

> [!bug] "I can't see my changes after committing"
> Run `git log` to confirm the commit exists. Your changes are committed — `git status` shows clean because there's nothing new to commit.

---

## Exercises

1. Create a file. Run `git status`. Notice "Untracked".
2. `git add` it. Run `git status`. Notice "Staged".
3. Commit it. Run `git status`. Notice "Clean".
4. Edit the file. Run `git status`. Notice "Modified".
5. Stage it. Edit it again. Run `git status` — notice it appears in BOTH sections.

---

## Related Notes

- [[05 Staging and Committing]] — Deep dive into `add` and `commit`
- [[06 Viewing History]] — See your commits
- [[07 Undoing Things]] — Undo operations at each state

---

## Official References

- [Pro Git: The Three States](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F#_the_three_states)
- [Pro Git: Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

---

#git/beginner #git/concepts #git/core
